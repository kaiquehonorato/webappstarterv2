## What changed
<!-- One or two sentences on what this PR does and why. -->

Closes #

## How it was verified
<!-- The commands you ran and their results; test names; the platforms and devices each check ran on, and the ones that could not run here. Phase 3 replaces these commands with the real ones. -->
```bash
pnpm typecheck && pnpm lint && pnpm test && pnpm e2e
```

## Screenshots
<!-- For UI changes: iOS and Android, light and dark, and one at the largest text size. Resized to 390 px wide. Site pages at 390 and 1280 px. -->

## Release impact
- [ ] Needs a new store build (native code, SDK, permission, entitlement, app config, icon or splash)
- [ ] Could ship over the air (JavaScript and assets only, nothing that changes what the app does)
- [ ] Changes what data the app collects or shares (the store privacy forms and the data inventory in SPEC.md change with it)
- [ ] Changes the API in a way old app versions notice (versioned, and the minimum supported version plan is below)

## Definition of done
- [ ] Every acceptance criterion in the Issue is met, with evidence above
- [ ] Tests were written first and failed before the change; none was skipped, weakened or deleted
- [ ] CI is green: types, lint, unit, component and integration tests, end-to-end flows, security scans and the size limits
- [ ] Every new endpoint has a "user A cannot access user B's data" test
- [ ] `/code-review` and the verifier ran; for auth, data, APIs, uploads, payments, permissions, deep links, SDKs, AI features or dependencies, `/security-review` and the security-reviewer too
- [ ] UI changes: screenshots on both platforms, the ui-reviewer ran, and docs/manual/ is updated if a use case changed
- [ ] New dependencies, SDKs and assets are listed with their licenses (assets in docs/assets-licenses.md) and the data they collect
- [ ] No secrets or real personal data in code, the app, logs, fixtures or screenshots
- [ ] docs/ROADMAP.md is updated, and docs/architecture.md or an ADR if the structure changed

## What's left or risky
