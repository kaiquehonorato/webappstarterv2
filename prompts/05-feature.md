# 05 · Build one feature
Model: Sonnet · effort medium; high for bug fixes; Opus · high or xhigh for a bug that survived two attempts and for money, permission, sync or concurrency logic · one new session per feature, a worktree when running features in parallel · on your computer when the feature needs the simulators
Start: `claude --model sonnet --effort medium`

Type `/new-feature <issue-number>`. The steps live in `.claude/skills/new-feature/SKILL.md`, the only copy, so the command and the instructions can't drift apart: understand the Issue, plan (and say whether it needs a new store build), branch, tests first, implement from the API to the screen, verify on iOS and Android, review (`/code-review`, the verifier and, where they apply, the security and UI reviewers), update the manual, open the PR, report.

Without the slash command (another tool, or to adapt the steps once), paste this instead:

```text
Follow .claude/skills/new-feature/SKILL.md for GitHub Issue #<N>.
```

When it gets hard:
- One hard step: add the word `ultrathink` to that message for deeper reasoning on that turn only.
- A bug that survived two attempts, or money, permission, offline sync or concurrency logic: start the session on Opus at high or xhigh effort (docs/models-and-tokens.md), or keep Sonnet and turn on the advisor with `/advisor opus`.
- A bug that only happens on one platform, one phone or a release build: reproduce it there first, with the systematic-debugging skill, before changing code. A simulator is not a phone.
- Two failed corrections: start a fresh session with a better prompt.

Features reach people only through a release. Merged PRs wait on main until `/store-release` ships them, so tell me when enough has merged to be worth a release.
