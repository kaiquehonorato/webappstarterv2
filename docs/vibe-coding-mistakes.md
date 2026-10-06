# The 52 most common vibe-coding mistakes, and the guardrail for each

"Vibe coding" (Andrej Karpathy, February 2025) means accepting AI-written code without reading it. It is great for throwaway experiments. For a product with real users it fails in predictable ways, and by 2026 the practice that replaced it is often called agentic engineering: the AI writes most of the code, while a person owns the plan, the checks and the review.

The numbers explain why the checks matter:
- Veracode's GenAI Code Security reports (2025, and again in 2026): in roughly 45% of coding tasks, models introduced a known vulnerability, and they failed most cross-site scripting and log injection cases.
- CodeRabbit (December 2025, 470 pull requests): AI-co-authored PRs had about 1.7 times more issues than human-only PRs, with security issues up to 2.74 times higher.
- METR's randomized trial (July 2025): experienced developers using AI were 19% slower on real tasks while believing they were 20% faster. How fast it feels is not evidence.

Each item below says what goes wrong and where this kit prevents it. Items 1–40 apply to any product; 41–52 are the ones a store app adds. Sources are in docs/REFERENCES.md.

## A. Planning and scope

**1. Coding before deciding what to build.** Without a spec, the AI fills the gaps with guesses, and every guess becomes code you later pay to remove.
Guardrail: Phase 0 interview, then SPEC.md with non-goals and Given/When/Then acceptance criteria (prompts/00-kickoff.md).

**2. Asking for the whole app in one prompt.** The result is wide and shallow: many screens, few that work, no tests.
Guardrail: thin vertical slices, one GitHub Issue per session (prompts/02-architecture.md, Part C).

**3. Letting the AI choose the stack, hosting and database by default.** Defaults bring lock-in, pricing cliffs and data stored in another country without a legal transfer basis.
Guardrail: Phase 2 options tables with cost at three sizes, region, backups and exit plan; your decision recorded as an ADR.

**4. Shipping the prototype as the product.** Demo code has no tests, no authorization and fake assumptions baked in.
Guardrail: the Phase 1 prototype lives in prototype/, is marked throwaway, and an architecture rule fails the build if the app imports from it.

**5. Building for scale on day one, or with no structure at all.** Microservices, Kubernetes and fifteen tools slow a solo builder down; a single giant file makes every change risky.
Guardrail: a modular monolith by feature, and a "later" list in docs/ROADMAP.md with the trigger for each tool (prompts/03-foundation.md).

**6. Not understanding your own code.** If nobody can explain how the app works, nobody can fix it at 2 a.m. Anthropic's advice for production vibe coding is to let AI write the "leaf" features freely while people review the core architecture.
Guardrail: plain-language explanation at every gate, ADRs, docs/architecture.md with diagrams, and one example feature that shows every layer.

## B. Working with the agent

**7. Kitchen-sink sessions.** One long session for unrelated tasks fills the context with noise, and quality drops as the context fills.
Guardrail: one task per session, /clear between tasks, fresh session per phase (docs/models-and-tokens.md).

**8. Correcting the same mistake over and over.** Each failed attempt stays in context and pulls the next attempt toward it.
Guardrail: after two failed corrections, start a fresh session with a better prompt (AGENTS.md).

**9. Vague prompts.** "Make it better" triggers broad, expensive exploration and changes you didn't want.
Guardrail: every prompt in this kit states the outcome, the constraints, the files to read and the check that proves success.

**10. No way for the AI to check its work.** Without a test, build or screenshot, "looks done" is the only signal, and you become the tester.
Guardrail: tests first, a Stop hook that runs the fast checks before Claude stops, `/run` or `/verify` against the running app, screenshots from both platforms for UI, and a statement of which checks ran on a real phone.

**11. Accepting changes without reading them.** Clicking "accept" on large diffs is how bugs and backdoors get in unnoticed.
Guardrail: small PRs (one Issue each), `/code-review` and a fresh-context verifier subagent on every PR, reviewers asked for evidence instead of opinions, and a definition of done in the PR template.

**12. Believing "done", "fixed" or "secure" without evidence.** Models report success confidently. In the Replit incident (July 2025) an agent even produced fake data that hid what it had done.
Guardrail: AGENTS.md requires commands and their output, test names or screenshots for every claim, and a statement of what wasn't verified.

**13. Instruction files so long the model ignores them.** A bloated CLAUDE.md or AGENTS.md buries the rules that matter.
Guardrail: AGENTS.md kept short; area rules in `.claude/rules/` load only for matching files; workflows live in skills.

**14. Installing plugins, MCP servers and skills nobody vetted.** They run with your permissions. Plugin4Shell (disclosed September 2026) showed a plugin's pinned code could be swapped after review.
Guardrail: docs/plugin-safety-checklist.md, official marketplace first, only the plugins the current phase needs; the skills the kit relies on are copied into the repo, read and pinned to a version, instead of installed (docs/skills-and-plugins.md).

**15. One model and effort level for everything.** Top model at maximum effort on every rename burns the budget; the smallest model on architecture produces confident nonsense.
Guardrail: a model and effort recommendation for every phase, and the rule "skipped work → more effort; missing knowledge → bigger model" (docs/models-and-tokens.md).

**16. Letting the agent weaken tests or hardcode answers.** A green run achieved by deleting assertions or special-casing test inputs hides a broken feature.
Guardrail: AGENTS.md forbids it, and the verifier subagent checks for skipped, weakened or deleted tests.

**17. No git discipline.** Without branches and commits there is no safe point to return to. Claude Code checkpoints don't capture changes made by shell commands, so they don't replace git.
Guardrail: Issue → branch → PR for every task, Conventional Commits, and main protected: by a pre-push hook from Phase 3, then by a GitHub ruleset once the account is on Pro (a private repository on GitHub Free can't use one).

## C. Code quality

**18. Business logic in the UI and database calls from the browser.** Rules scattered across components can't be tested, and anything the browser can call, an attacker can call too.
Guardrail: controller → service → repository layers, enforced by architecture rules in CI (AGENTS.md, prompts/02-architecture.md).

**19. Copy-pasted components and styles.** Five slightly different buttons mean five places to fix every bug and an inconsistent product.
Guardrail: one design system in packages/ui with every state, used everywhere (prompts/04-design-system.md), and a cleanup review every 5–10 features that finds the duplicates and dead code that slipped through (prompts/09-cleanup-review.md).

**20. Tests that prove nothing.** Tests that mock everything, or that would still pass with the feature deleted, give false confidence.
Guardrail: the test-writer subagent tests behavior against a real database, confirms tests fail first, and includes hostile inputs.

**21. Hallucinated, look-alike or abandoned dependencies.** A USENIX Security 2025 study of 576,000 code samples found that about one in five suggested packages did not exist (5.2% for commercial models, 21.7% for open-source ones), and in a rerun test 43% of the invented names came back every time. Attackers register such names ("slopsquatting").
Guardrail: before any install, confirm the exact name on the official registry, maintenance, popularity and license; lockfile committed; new versions wait a day before they install, because most malicious releases are found and pulled from the registry within an hour (pnpm's documentation); install scripts run only for approved packages; Renovate or Dependabot propose updates after a cooldown.

## D. Security

**22. Secrets in code, the browser bundle or git history.** In January 2026 Moltbook exposed its whole production database because a key sat in client-side JavaScript with no database rules behind it.
Guardrail: env vars validated at startup, `EXPO_PUBLIC_*`, `NEXT_PUBLIC_*`, `VITE_*` and everything inside the app treated as public (#42), secret scanning in the pre-commit hook, in CI and in the built app, `.env` files and signing keys blocked from the agent.

**23. Checking permissions only in the UI or middleware.** CVE-2025-29927 (CVSS 9.1) let a single request header skip Next.js middleware, and with it every authorization check that lived only there.
Guardrail: authorization is enforced in the service and repository too; skipping middleware must expose nothing (AGENTS.md, backend rules).

**24. IDOR: change an ID in the URL, see someone else's data.** This is the top risk in both the OWASP Top 10:2025 (Broken Access Control) and the API Security Top 10.
Guardrail: every query scoped by owner or tenant, 404 for records the user may not see, and a "user A cannot access user B" test for every endpoint, with a CI check that fails when a route has no such test, so the rule can't be forgotten on the route that matters (prompts/03-foundation.md, Step 3).

**25. Database or storage open to the browser without rules.** Lovable's CVE-2025-48757: 170 of 1,645 scanned generated apps let anyone read or write their tables. The Tea app (July 2025) leaked about 72,000 images, including ID photos, from storage with misconfigured rules.
Guardrail: row-level security on every table and private buckets with signed URLs, each covered by tests, or no direct client access at all.

**26. Building SQL, HTML or shell commands from strings.** This is still how injection and XSS happen, and AI models fail these cases often.
Guardrail: parameterized queries only, a sanitizer for any user HTML, CSP with nonces (hashes on a static host), and static-analysis rules in CI that fail on string-built SQL, raw HTML and eval.

**27. Validating input only in the browser.** Anyone can send a request without your form.
Guardrail: the shared zod schema runs on the server for every request; the browser check is only for user feedback.

**28. "Hidden" routes as security.** An unlinked admin page or an unguessable URL is found through bundles, logs, history or brute force. Obscurity is not access control.
Guardrail: every route is protected as if it were public; no debug routes, stack traces or public source maps in production (docs/security-model.md).

**29. Misconfiguration and failing open.** Verbose errors, CORS `*` with credentials, missing security headers, and code that grants access when a check throws. Misconfiguration is #2 in the OWASP Top 10:2025, and "Mishandling of Exceptional Conditions" is new at #10.
Guardrail: headers, cookies and CORS as code with a test; fail closed; generic error messages.

**30. AI features without limits.** Prompt injection through user or web content, model output rendered as HTML, tools with too many permissions, and no cap on spend per user.
Guardrail: the AI section of the security audit (OWASP Top 10 for LLM Applications 2025, Top 10 for Agentic Applications 2026); per-user quotas.

**31. Giving the agent production access.** In July 2025 a Replit agent ran destructive commands against a production database during a declared code freeze. Replit's fixes included automatic separation of development and production databases.
Guardrail: no production credentials in agent sessions; `ask` rules for destructive database commands and production deploys in .claude/settings.json; a fresh backup before risky changes; two-step sign-in on every account that can reach production (docs/launch-checklist.md).

## E. Operations

**32. No limits.** No rate limits, quotas, page-size caps or budget alerts means one bot, bug or viral moment can take the app down or empty your wallet.
Guardrail: rate limits on auth and expensive endpoints, hard pagination maximums, quotas on AI features, budget alerts on every paid service.

**33. No backups, or a restore nobody tested.** A backup you have never restored is a hope, not a plan.
Guardrail: a backup plan sized to the data you can afford to lose (point-in-time recovery only when a day of data matters), files backed up as well as the database, and a timed restore test before launch, then every quarter (prompts/08-launch.md).

**34. Risky database migrations.** Editing a migration that already ran, or dropping a column in the same release that stops using it, breaks production.
Guardrail: reversible migrations reviewed in the PR, expand → migrate → contract, never run from a development session against production.

**35. Flying blind, or logging personal data.** Without error monitoring you learn about outages from users; logs full of e-mail addresses and PPS numbers create a leak of their own.
Guardrail: error monitoring with personal data scrubbing, structured logs with redaction, alerts that reach a person.

**36. No CI gate, deploying or building releases from a laptop.** "Works on my machine" reaches production, or a store, without tests or scans, built with whatever was on that laptop.
Guardrail: CI required before merge, preview deploys and preview builds per change, production deploys only from main through the pipeline, store builds only from the pipeline's production profile.

## F. Product quality

**37. Only the happy path.** No loading, empty or error states, no thought about slow or missing networks, double taps or bad input.
Guardrail: loading, empty, error, success and offline states for every async screen, undo or named confirmation for destructive actions, the edge cases written into the acceptance criteria.

**38. The generic "AI look" and AI-sounding text.** The same fonts, gradients, cards and hype words make a product look like every other generated site, and users notice.
Guardrail: 2–3 reference screenshots you chose, three visual directions grounded in the product, a named list of looks to avoid, the frontend-design skill in .claude/skills/, and docs/writing-style.md for every piece of text.

**39. Ignoring accessibility and performance on real phones.** Small touch targets, text that ignores the phone's text size, controls a screen reader can't name, low contrast, gestures with no alternative, slow starts and lists that stutter shut people out, and in the EU accessibility is also law for many consumer services (the European Accessibility Act, since June 2025).
Guardrail: WCAG 2.2 AA applied to native screens (VoiceOver and TalkBack, text up to 200%, targets of 44 pt and 48 dp, a visible alternative to every gesture), screenshots on both platforms at the largest text size, performance budgets measured on a low-end Android phone with size limits in CI, motion off under the phone's reduce-motion setting, and the web rules for the store pages (prompts/04-design-system.md, prompts/04b-public-site.md).

## G. Legal

**40. Leaving privacy and legal for later.** Collecting data without a purpose or legal basis, no way to delete or export it, SDKs and cookies before consent, children without the protections the GDPR and the DPC's Fundamentals expect, data outside the EEA without a transfer mechanism, nothing ready for the 72-hour breach notice or the Cyber Resilience Act's 24-hour warning, assets used without a license.
Guardrail: the data inventory from Phase 0, the legal rules in AGENTS.md, the Phase 7 review, and a real lawyer before launch.

## H. Store apps

**41. Building a store app when a web app would do.** Two platforms, store fees, a review before every release and users who update weeks late, for an idea that a responsive web app would have served on every phone the same day.
Guardrail: Phase 0's reality check asks this first and points to the web app starter kit when the answer is no; Apple also rejects apps that are mainly a repackaged website (Guideline 4.2).

**42. Secrets in the app.** Every string in an app binary can be read in minutes. In September 2022 Symantec found 1,859 Android and iOS apps with hard-coded AWS credentials, many inherited from a shared SDK; in five banking apps they exposed more than 300,000 people's fingerprint data.
Guardrail: nothing secret in the app (AGENTS.md); public prefixes and config files treated as public; a check that fails the build when a secret-looking value is about to be compiled in; the built app unpacked and scanned before every release (prompts/03-foundation.md, Step 4).

**43. Trusting the app to enforce the rules.** A price, a role, a "premium" flag or a check that lives in the app is a suggestion: anyone can call the API without the app, or patch it.
Guardrail: the app is an untrusted client; the same per-record authorization, validation and subscription checks run on the server for every request (backend rules), and the audit's section M tests the app's side.

**44. Tokens and personal data in plain storage.** AsyncStorage, SharedPreferences, UserDefaults and plain files are readable from backups, rooted phones and some debugging tools.
Guardrail: tokens and sensitive data only in Keychain or Keystore-backed storage, excluded from backups, cleared on sign-out; a static-analysis rule that fails on tokens written elsewhere (.claude/rules/mobile.md).

**45. No way to make people update.** Without a minimum supported version, a security bug or a breaking API change stays live on every phone that never updates.
Guardrail: the API answers versions below the minimum with "update required" and the app shows an update screen, set up in Phase 3; API changes stay additive so old versions keep working until the minimum moves (SECURITY.md).

**46. Losing control of the app's identity.** A lost Android upload key without Play App Signing, a store account in a freelancer's name, or a bundle ID chosen in a hurry: the first two can stop you from ever updating the app, and the third can never be changed.
Guardrail: Play App Signing on, keys kept in the build service and backed up by you, never in the repo or an agent session; accounts owned by the product's owner with two-step sign-in; the identifiers decided with you in Phase 3 (docs/launch-checklist.md).

**47. Meeting the store rules at submission time.** Account deletion missing, a privacy policy link that doesn't load, privacy forms that don't match the app, no demo account for the reviewer, vague permission texts: each one is a rejection and another wait.
Guardrail: docs/app-stores.md read in Phase 0, the rules turned into Issues in Phase 2, the required pages in Phase 4b, the forms drafted in Phase 7, the review notes in Phase 8.

**48. Leaving Google's closed test for launch week.** A personal Google Play account created after November 2023 needs 12 testers opted in for 14 days in a row before it can publish, and the production access review comes after that.
Guardrail: Phase 0 asks which accounts you have, and Phase 3 starts the test with the first preview build, so it ends long before launch (docs/ROADMAP.md tracks the store clocks).

**49. Testing only on a simulator, one platform or a new phone.** Simulators hide performance problems, notification and camera behavior, and release-build differences; a flagship phone hides what most users experience.
Guardrail: every UI change checked on iOS and Android; release builds tested on a real iPhone and a low-end Android phone before every submission (`/store-release`); performance budgets measured on the slowest phone in the device list.

**50. Asking for every permission at launch.** People refuse a wall of system prompts, and after a refusal the system often won't show the prompt again, so the person has to find the setting themselves; reviewers reject vague purpose texts.
Guardrail: each permission asked at the moment its feature needs it, after a screen that says why, with a path forward when it is refused, and purpose strings that say what the app does with the access (.claude/rules/mobile.md, docs/writing-style.md).

**51. SDKs that collect data nobody declared.** Analytics, ads and crash SDKs send data of their own, which the store privacy forms and the privacy policy must declare. Forms that don't match the app are a store policy violation and a GDPR problem.
Guardrail: every SDK approved with its data listed (AGENTS.md); the forms drafted from the data map and checked against a capture of the app's real traffic (prompts/06-security-audit.md, check 95; prompts/07-legal-review.md).

**52. Shipping over the air what the store didn't review.** Over-the-air updates are allowed for fixes and changes within what was reviewed; a new feature that changes what the app does breaks Apple's and Google's rules, and an unsigned update channel is a supply chain risk.
Guardrail: `/store-release` decides between a store build and an over-the-air update and lists the reason; updates are signed, published to a preview channel first, and matched to the native runtime they were built for.
