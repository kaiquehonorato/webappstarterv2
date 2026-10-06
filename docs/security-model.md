# Security model

No real system is impenetrable, and an AI that says otherwise is telling you what you want to hear. What a careful team does instead: remove whole classes of bugs by design, check every change automatically, check again on a schedule, notice attacks, and recover quickly. In this project every "secure" claim points to a control and the test or scan that proves it.

## The one idea that changes for a phone app
Your app runs on a phone you don't control. Anyone can download it, unpack it, read every string and every endpoint in it, run it on a rooted or jailbroken phone, change it, or skip it entirely and call your API with a script. So the app is only another client: it can make things pleasant, but it can't keep a secret or enforce a rule. Every secret stays on the server, every rule is enforced on the server, and the phone keeps only what the person needs, in the platform's secure storage.

And unlike a website, you can't fix every copy at once. Installed versions stay in use for months. That is why the kit has a minimum supported app version the server enforces, API changes that old versions survive, a server-side switch to turn a feature off, and staged rollouts.

## What people ask for, and what works

| The wish | Why it fails as stated | What this kit does |
|---|---|---|
| Hidden routes | Unlinked URLs leak through JavaScript bundles, logs, browser history, referrers and guessing. Obscurity is not access control. | Every route is authenticated and authorized as if it were public. Records a user may not see return 404. No debug or admin routes in production, no public source maps, and admin-only code is never sent to other users. |
| A key hidden inside the app | Every string in an app binary can be read in minutes, obfuscated or not. | No secret in the app at all. The server holds the keys and calls the services that need them; the pre-release scan of the built app looks for anything that slipped in. |
| "Only our app can call our API" | A script or a modified app can send exactly the same requests. | The API authenticates the person and authorizes every record, whatever client calls it. Where abuse justifies it, App Attest or Play Integrity add a signal that the request came from a genuine copy of the app, checked on the server. |
| Root and jailbreak detection as protection | Detection can be bypassed by whoever controls the phone. | Treated as a risk signal for high-stakes apps, never as the control. The controls stay on the server. |
| SQL injection made impossible | Absolute guarantees don't exist, but the cause can be designed out. | Parameterized queries through the ORM only; CI fails on SQL built from strings; the app's database user has only the privileges it needs; tests send hostile input. |
| Code injection made impossible | XSS, command and template injection come from mixing data with code. | Framework auto-escaping; a sanitizer for any user HTML; CSP with nonces, or hashes on a static host; no eval; no shell commands built from input; CI rules that fail the build on each. |
| An app that tests itself | Achievable, in layers. | The schedule below. |

## Layers
Each layer assumes the one before it will miss something.
1. **Design** (Phase 2): a threat model for each use case, including a lost or rooted phone, other apps on the same phone and a script calling the API; deny by default; least privilege; development and production kept apart, down to separate app variants.
2. **While coding**: the rules in AGENTS.md and .claude/rules/, tests written first (authorization tests included), a Stop hook that runs the fast checks before Claude stops, and the security-guidance plugin if you installed it.
3. **Before the PR**: `/code-review`, `/security-review`, the security-reviewer subagent, the verifier subagent.
4. **Every PR, required to merge**: static analysis with custom rules (including tokens written to non-secure storage and WebView bridges), dependency and license audit, secret scan, unit, component, integration and end-to-end tests, the authorization tests and the check that every record route has one, the security-headers test, the size limits, the token contrast test.
5. **Before each release**: the built app unpacked and scanned for secrets, internal URLs and debug flags; the Android manifest and Info.plist checked; the store privacy forms compared with what the release really collects (`/store-release`).
6. **On every install**: a new package version must be a day old, and only approved packages run install scripts.
7. **On a schedule**: a weekly DAST baseline scan, signed in past the preview's sign-in page, plus a dependency audit and a Lighthouse run; a monthly Claude Security scan of the month's changes; a quarterly restore test; a full audit (prompts/06-security-audit.md) every 3–5 features.
8. **In production**: rate limits, monitoring and alerts, crash and ANR alerts for the app, security events logged without personal data, the minimum supported app version, staged rollouts, backups sized to the data loss SPEC.md accepts, with files covered too.
9. **People and process**: MFA for admins, two-step sign-in on every account you own, store accounts owned by the product's owner, signing keys out of everyone's reach (docs/launch-checklist.md), secret rotation, the incident plan in SECURITY.md, the legal review, and a professional security review of the app and the API before launch when the product handles payments, health or ID data, or users under 18.

## What `.claude/settings.json` does, and what it doesn't
- It stops Claude's file tools from reading `.env` files, key files, signing keys and store credentials, blocks force-pushes, pushes to main and `rm -rf`, and makes Claude ask before destructive or production commands, store submissions and over-the-air updates.
- These rules match the command text Claude usually writes. They are not a security boundary: `bash -c '…'`, `/bin/rm`, or a script that opens `.env` by itself gets past them.
- For a boundary the operating system enforces, turn on the sandbox with `/sandbox` (macOS, Linux and WSL2) and list your `.env` files under `sandbox.filesystem.denyRead`.
- What holds either way: no production credentials on your computer or within Claude's reach, every change committed to git, branch protection on main, and backups. On GitHub Free a private repository can't protect main, so until you move to GitHub Pro a pre-push hook refuses pushes to main (docs/launch-checklist.md).

## Self-testing schedule

| When | What runs | Tool (example) | Blocks the merge? |
|---|---|---|---|
| Each time Claude stops with changed files | Typecheck, lint, unit tests of the changed package | Stop hook (Phase 3) | Claude fixes it in the session |
| Each edit by Claude, optional | Dangerous-pattern warnings, diff review at the end of each turn | security-guidance plugin (terminal) | Claude fixes it in the session |
| Each install | New versions wait a day; install scripts need approval | pnpm settings (Phase 3) | Yes (the install) |
| Each commit | Secret scan | gitleaks in the pre-commit hook | Yes (the commit) |
| Each push | Pushes to main refused | pre-push hook, then a GitHub ruleset on Pro | Yes (the push) |
| Each PR | Types, lint, unit, component and integration tests, end-to-end flows on Android, authorization tests and the route-coverage check behind them, static analysis, dependency, license and secret scans, headers test, size limits, contrast test | CI | Yes |
| Each PR | Bug review of the diff | `/code-review` | Findings fixed before merge |
| Each PR touching sensitive code | Security review of the diff | `/security-review`, security-reviewer subagent | Findings fixed before merge |
| Each release | Scan of the built app, manifest and Info.plist checks, privacy forms against the release, test build on real phones | `/store-release` and the Phase 3 script | Yes (the submission) |
| Weekly | DAST baseline scan of the API and the site on preview or staging, signed in with a test account; Lighthouse run on the site; iOS end-to-end flows if they don't run per PR; full dependency audit | OWASP ZAP, Lighthouse CI, the end-to-end tool, the package manager's audit | Opens Issues |
| Monthly | Deep scan of the month's changes | Claude Security plugin | Opens Issues |
| Every 3–5 features and before launch | Full manual and automated audit | prompts/06-security-audit.md | Fix plan approved by you |
| Quarterly | Restore a backup and time it | Runbook | – |

## Standards behind the checklists
- OWASP Top 10:2025: A01 Broken Access Control, A02 Security Misconfiguration, A03 Software Supply Chain Failures, A04 Cryptographic Failures, A05 Injection, A06 Insecure Design, A07 Authentication Failures, A08 Software or Data Integrity Failures, A09 Security Logging and Alerting Failures, A10 Mishandling of Exceptional Conditions.
- OWASP MASVS 2.1 (with MASTG for how to test it) for the app: storage, cryptography, authentication, network, platform interaction, code quality, resilience and privacy.
- OWASP Mobile Top 10 (2024): M1 Improper Credential Usage, M2 Inadequate Supply Chain Security, M3 Insecure Authentication/Authorization, M4 Insufficient Input/Output Validation, M5 Insecure Communication, M6 Inadequate Privacy Controls, M7 Insufficient Binary Protections, M8 Security Misconfiguration, M9 Insecure Data Storage, M10 Insufficient Cryptography.
- OWASP ASVS 5.0, Level 2 as the default target for the API.
- OWASP API Security Top 10 (2023).
- OWASP Top 10 for LLM Applications 2025 and Top 10 for Agentic Applications 2026, when the product has AI features.
- OpenSSF Security-Focused Guide for AI Code Assistant Instructions: specific, short security instructions and self-review work better than telling the model it is a security expert.

## The common 50-point checklist
A 50-point security checklist for AI-built apps circulates widely. Every item in it is a check in the audit (prompts/06-security-audit.md); the letter is the audit's section and the number its check. Section M (checks 84–103) adds what the list leaves out for an app on a phone. The circulating list skips number 33, and item 32 is cut off; here it reads "only on the client".

| # | Item | Audit check |
|---|---|---|
| 1 | Exposed database credentials | E43, D40 |
| 2 | Public .env files | D38 |
| 3 | Hardcoded API keys | E43, M84 |
| 4 | Weak or missing authentication | A1, B15 |
| 5 | No authorization checks | A1, A2, A3 |
| 6 | Users able to access other users' data | A2 |
| 7 | Open database read/write permissions | A7, D40 |
| 8 | Misconfigured Firebase / Supabase / S3 buckets | A7, A9 |
| 9 | Admin routes left unprotected | A3, A11 |
| 10 | Debug pages exposed in production | A11, D37 |
| 11 | Build logs leaking secrets | E45 |
| 12 | Verbose error messages leaking stack traces | D37 |
| 13 | Leaked GitHub repos or commit history | E43, E46 |
| 14 | Secrets included in frontend JavaScript | E44, M84 |
| 15 | Client-side-only security checks | A6, M89 |
| 16 | Missing input validation | C26 |
| 17 | SQL injection | C27 |
| 18 | NoSQL injection | C28 |
| 19 | Cross-site scripting (XSS) | C30 |
| 20 | Cross-site request forgery (CSRF) | B20 |
| 21 | Insecure file uploads | F53 |
| 22 | Path traversal | C32 |
| 23 | Server-side request forgery (SSRF) | A12 |
| 24 | Broken password reset flows | B21 |
| 25 | Weak session management | B18 |
| 26 | Weak, leaked or reused JWT secrets | B17 |
| 27 | Overly permissive CORS | D36 |
| 28 | Missing rate limits on login, signup, APIs and AI endpoints | F48 |
| 29 | Public test or staging environments | D42 |
| 30 | Default credentials left unchanged | B25 |
| 31 | Webhooks without signature verification | F51 |
| 32 | Payment or subscription checks only on the client | A14 |
| 34 | API endpoints that trust user-controlled IDs or roles | A4 |
| 35 | Logs containing tokens, e-mails, passwords or private data | J70, M86 |
| 36 | Source maps exposed in production | D38 |
| 37 | Dependency vulnerabilities | H58 |
| 38 | Outdated packages | H59 |
| 39 | Prompt injection in AI features | K76 |
| 40 | AI tools or actions without permission checks | K79 |
| 41 | Excessive database permissions for the app user | D41 |
| 42 | No audit logs | I68 |
| 43 | No monitoring or alerting | I69 |
| 44 | No backup or restore plan | L81 |
| 45 | Publicly exposed internal dashboards | A3 |
| 46 | Missing security headers | D35 |
| 47 | Cookies missing HttpOnly, Secure or SameSite | B19 |
| 48 | Unencrypted sensitive data | E47, M85 |
| 49 | Poor tenant isolation | A8 |
| 50 | Over-trusting generated code without review | H66 |

## Lessons from real incidents

| Incident | What happened | The rule it produced here |
|---|---|---|
| Moltbook, January 2026 | A database key in client-side JavaScript and no row-level security exposed the whole production database, including 1.5 million API tokens | Row-level security on every table with tests, or no direct client access |
| Tea app, July 2025 | Misconfigured storage rules exposed about 72,000 images, including ID photos | Private buckets, signed URLs that expire |
| Symantec's study, September 2022 | 1,859 Android and iOS apps shipped hard-coded AWS credentials, often inside a shared SDK; in five banking apps they exposed more than 300,000 people's fingerprint data | No secret in the app; every SDK vetted and its data declared; the built app scanned before each release |
| Lovable, CVE-2025-48757 | 170 of 1,645 scanned generated apps let anyone read or write their tables | The same, plus security checks in the generator's own output |
| Next.js, CVE-2025-29927 | One request header skipped middleware and every check that lived only there | Authorization enforced in the service and repository |
| Replit and SaaStr, July 2025 | An agent ran destructive commands on a production database during a code freeze | Agents never hold production credentials; ask rules for destructive commands; backups |
| Plugin4Shell, September 2026 | Plugin code could be swapped after it was reviewed and pinned | Few, vetted plugins; Claude Code kept up to date |

## What stays at risk
Unknown flaws in dependencies, SDKs, the phone's operating system or the hosting platform; mistakes in business rules that no test covers; old app versions that people never update; stolen admin, owner or store account credentials; and social engineering. That is why monitoring, the minimum supported version, backups, two-step sign-in and the incident plan exist, and why the audit ends with a plain statement of what it could not cover.
