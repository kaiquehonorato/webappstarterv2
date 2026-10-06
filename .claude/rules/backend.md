---
paths:
  - "apps/api/**"
  - "packages/db/**"
  - "**/api/**"
  - "**/service/**"
  - "**/repository/**"
  - "**/server/**"
  - "**/migrations/**"
  - "**/*.sql"
---

# Backend rules
Loaded only when Claude reads matching files. Update the `paths` above in Phase 2 to match the real folders.

- Controller order: authenticate → validate input (zod schema from `packages/shared`) → call one service method → map the result or a typed domain error to HTTP. No business rules or SQL in controllers.
- The service authorizes the action on this specific record; the repository scopes every query by owner or tenant. Middleware may reject early, but skipping it must expose nothing. Never trust an ID from the client alone: user, role, tenant and plan come from the session, and paid features check the subscription on the server from the payment provider's or the store's verified state (server notifications or receipt validation), never from a flag the app sends.
- The app is not a trusted client. Anyone can call the API with a script or a modified app, so every rule the app shows is enforced here too.
- Writable fields come from an explicit allow-list; never spread a request body into a model (mass assignment).
- Tokens for the app: short-lived access tokens, refresh tokens that rotate on use and can be revoked per device, all revoked on sign-out, password change and account deletion. Cookie sessions (the site, a web version): CSRF protection on every state-changing route. GET never changes state.
- Old app versions keep calling the API for months. Changes are additive; a breaking change gets a new version of the endpoint, and the old one stays until the minimum supported app version passes it. Every request carries the app version and platform, and the API answers versions below the minimum with a typed "update required" error the app turns into the update screen.
- Parameterized queries or the ORM only. Raw SQL only through the ORM's parameter-binding template, reviewed in the PR; never the "unsafe" or string-concatenating APIs.
- Errors: one JSON shape `{ error: { code, message } }`. No stack traces, SQL, internal IDs or hints about whether an account exists. When a check fails or throws, deny (fail closed).
- Logging: structured, with a request ID. Never log passwords, tokens, push tokens, PPS numbers, e-mail, phone, address, precise location or request bodies with personal data. Strip line breaks from user input before logging it.
- Push notifications: device tokens are personal data, stored per user and device, deleted on sign-out and account deletion; payloads carry no private content; sending is rate-limited per user.
- Account deletion: one service method that deletes or anonymizes the person's data everywhere (database, file storage, push tokens, third parties), called by the app's in-app flow and by the web request page, with a test.
- Migrations: one per change, reviewed in the PR, reversible; expand → migrate → contract for breaking changes; never edit a migration that has already run; never run one against production from a development session. The app connects with a least-privilege database role; migrations run with a separate one.
- Money in integer minor units (cents), times in UTC, no floats for money.
- Rate-limit auth and expensive endpoints per user and per device. Verify webhook and store server notification signatures and reject replays. Idempotency keys for payments, offline writes the app retries, and other retried writes. Set timeouts on every outbound call.
- Outbound HTTP to user-supplied URLs only through an allowlist that blocks private IPs and cloud metadata addresses (SSRF).
- Integration tests run against a real database in a container and include a "user A cannot access user B's record" test for every endpoint. CI enforces it: the route-coverage check fails when a route that touches a record has no such test, and a route meant to be public goes in that check's allow-list with a one-line reason, so it shows up in the PR diff.
