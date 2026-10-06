# 09 · Cleanup review: dead code, duplication, complexity
Model: Opus · effort high for the review (xhigh for a large or old codebase); the removal PRs run on Sonnet · effort medium · fresh session · every 5–10 features, before launch, and whenever small changes start taking too long
Start: `claude --model opus --effort high`

A read-only review first, then removals in small PRs you approve. It questions everything and deletes carefully: nothing goes without evidence that it's unused, and every removal can be reverted. In a store app it also looks at what the app carries onto people's phones: permissions, SDKs and assets cost download size, privacy-form entries and review questions.

```text
Act as a senior engineer reviewing this repository and the app it builds for code quality and maintainability. Read AGENTS.md, docs/architecture.md, the ADRs, docs/ROADMAP.md and docs/app-stores.md first. The goal is a smaller, simpler codebase that does the same things: remove anything that doesn't provide value. Be aggressive in what you question and careful in what you delete.

Don't change code during the review. Every finding needs evidence: a tool's output, a search that shows no references (dynamic ones included), or file:line.

Step 0. Automated sweep. Run the tools for our stack and collect the results (examples for TypeScript, Python equivalents in brackets): Knip for unused files, exports, types and dependencies [Vulture, deptry]; jscpd for duplicated code; the type checker and linter with unused-code and complexity rules on, such as ESLint complexity and sonarjs cognitive-complexity [Ruff, Radon]; dependency-cruiser for orphan modules and circular dependencies; the test coverage report, for code no test runs; a bundle analyzer, for heavy or unused app and site code; the app's size report from the stores or the build (download and install size, per architecture), and the list of permissions, entitlements, native modules and SDKs in the release build; and git history, for files untouched for months that nothing imports. With my OK, run the end-to-end tests with query logging and network capture on, to see the database queries and API calls each screen makes. Treat each result as a lead, not a verdict.

Step 1. Map what is alive. List every entry point: screens in the navigation config, deep link and app link paths, push notification handlers, background tasks, widgets, site routes, API endpoints, webhooks, store server notifications, scheduled jobs, workers, CLI scripts, and anything that a config file, a package.json script, a CI workflow or a framework convention (file-based routes, migrations) calls. An API endpoint or field is alive while any app version at or above the minimum supported version can call it, even if the current app doesn't. Code reachable from an entry point is alive; everything else is a suspect until shown to be used. Before calling anything unused, search for dynamic references too: string-built imports, lazy routes, dependency-injection registries, feature flags, env-gated code, templates, docs/manual and tests.

Step 2. Check each category:
1. Dead code: unused functions, files, components, routes, API endpoints, variables, imports, dependencies and env vars; feature flags that are fully on or off; commented-out code; TODOs older than three months.
2. Duplicate logic that belongs in one place: validation written again instead of using the packages/shared schemas, copied helpers, two clients for the same service, near-identical components.
3. Unused UI components and design tokens in packages/ui, and one-off components that duplicate one there.
4. Overly complex implementations: abstractions with one real use, deep inheritance, long functions with high cognitive complexity, options nobody sets, hand-written code for something the framework or an existing dependency already does.
5. Legacy code: old versions of flows kept "just in case", compatibility code for clients that no longer exist, one-off data fixes that still run, the prototype/ folder once the real app covers it.
6. Redundant database queries and API calls: N+1 queries, the same data fetched twice for one screen, missing batching, no caching for data that rarely changes, polling that a webhook could replace, unbounded queries.
7. Files that look abandoned or disconnected: scripts nobody runs, docs about removed features, sample or scratch files, assets nothing references (check docs/assets-licenses.md too).
8. Technical debt worth paying now: whatever slows down every change, such as missing tests around core rules, names that don't match docs/GLOSSARY.md, outdated dependencies, flaky tests, slow CI steps.
9. What the app carries onto phones: permissions and entitlements no feature uses; SDKs and native modules with one use or none; fonts, images, sounds and locales in the bundle that nothing shows; code for app versions below the minimum supported version; over-the-air update channels and runtime versions nobody is on; feature flags fully on or off. Each removed permission or SDK also changes the store privacy forms, so list the form changes with it.
10. Drift between the documents and the code: a use case in SPEC.md that nothing implements, a shipped behavior no document describes, acceptance criteria no test checks, an ADR the code stopped following, a screen docs/manual/ shows that no longer looks like that. Each one is a finding with a path, the same as dead code, and the fix is either the code or the document, never quietly neither.

Step 3. Output:
1. Summary: the size of the codebase and the app today (files, lines, dependencies, SDKs, permissions, app download size, CI and build time) and how much the plan would remove.
2. Findings table: # · category · path:line · why it's unnecessary · evidence · impact of removing it (lines, dependencies, app or bundle KB, build or CI time, privacy-form entries, less to maintain) · risk before deletion · confidence (high/medium/low) · action (remove / consolidate / simplify / remove after a check / keep, with the reason).
3. For each medium- or low-confidence finding: the check that would settle it, such as a log line or a feature flag left in place for one release before the removal.
4. Cleanup plan: small PRs in this order, one category each, every test green before and after: (1) unused imports, variables and dependencies; (2) unused files, exports and components with no dynamic references; (3) duplicates merged, behind tests that pin today's behavior; (4) simplifications, with characterization tests first where tests are missing; (5) legacy code, after its check; (6) query and API call fixes, with before and after measurements. Database tables and columns go last, through expand → migrate → contract.
5. What not to touch, and why: migrations that already ran; API endpoints and fields that supported app versions still call (raise the minimum supported version first, then remove them in a later cleanup); deep link paths that the site's association files publish or that are printed somewhere, such as a QR code; public APIs that others call (deprecate them first); code that a config file or framework convention loads; anything a security, store or legal control depends on.

Rules for the removal PRs: no change in behavior; one category per PR; the full test suite, the build and the end-to-end flows pass on both platforms; the PR lists what was removed, the evidence it was unused, how to revert it, and whether it needs a store build to reach people; docs/architecture.md, docs/ROADMAP.md and the store privacy forms change with it.

Wait for my approval of the plan before creating Issues or changing any code.
```

After you approve the plan, each cleanup PR is a GitHub Issue built in a fresh session: `claude --model sonnet --effort medium`, then `/new-feature <issue-number>`.
