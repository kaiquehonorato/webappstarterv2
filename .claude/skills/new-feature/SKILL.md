---
name: new-feature
description: Build one feature end to end from a GitHub Issue - plan, tests first, implement in the API and the app, check on iOS and Android, review, update the manual, open PR.
disable-model-invocation: true
argument-hint: "[issue-number]"
---
Build the feature for GitHub Issue #$ARGUMENTS, following AGENTS.md and the rules in .claude/rules/.

1. **Understand**: `gh issue view $ARGUMENTS` (in a browser session, where `gh` isn't signed in, read it with the session's GitHub tools). Read the use case it serves in SPEC.md and docs/manual/USE-CASES.md. Use the `explorer` subagent to find related code and the example feature's patterns. If the acceptance criteria are unclear, ask me up to 3 questions (each with a recommended default) before continuing.
2. **Plan**: files to create or change in the API and the app, data model changes, contract changes in `packages/shared` (additive, or a new endpoint version), screens with their states and copy, and anything native: a new permission, entitlement, SDK, config or asset. A native change means the feature can only reach people through a new store build, so say so. If the diff is bigger than one sentence, or touches anything on the "always ask" list in AGENTS.md, wait for my approval.
3. **Branch**: `git checkout -b feat/$ARGUMENTS-<slug>`.
4. **Tests first**: use the `test-writer` subagent; confirm the tests fail for the right reason. For a bug, find the root cause first with the systematic-debugging skill and start with a test that reproduces it. A change to appearance only needs screenshots, not a failing test.
5. **Implement** vertically: migration → repository → service → controller (authenticate, validate with zod, authorize this record) → typed API client and hook → screen (loading, empty, error, success and offline states with `packages/ui`; the frontend-design skill for anything new; platform conventions; copy per docs/writing-style.md, purpose strings included).
6. **Verify** (verification-before-completion): typecheck, lint, unit, component and integration tests, and the end-to-end flow for this use case. Run the app on an iOS simulator and an Android emulator, go through the flow on both, and take screenshots of the screens this change touched, in light and dark and once at the largest text size; resize them to 390 px wide and open only those. Without a simulator (a browser session), say which checks are left for my computer or CI. Fix root causes. Say "done" only about checks that ran in this session, with their output.
7. **Review**: run `/code-review` on the diff and the `verifier` subagent. If the change touches auth, data, APIs, uploads, payments, permissions, deep links, SDKs, AI features or dependencies, also run `/security-review` and the `security-reviewer` subagent. If it touches UI, run `ui-reviewer`. Fix real findings only.
8. **Manual**: follow `.claude/skills/use-case-manual/SKILL.md` for the use cases this feature changed.
9. **Ship**: Conventional Commit, push, open the PR with `gh pr create` or the session's GitHub tools (never merge into main locally). Fill in .github/pull_request_template.md: summary, test evidence per platform, screenshots, whether it needs a store build, the definition of done, and "Closes #$ARGUMENTS". Update docs/ROADMAP.md.
10. **Report**: what changed, the evidence, what's left (including device checks only I can run), and whether I should start a new session for the next task.
