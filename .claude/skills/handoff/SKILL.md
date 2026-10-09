---
name: handoff
description: Save where a long task stands - push the work, write a short handoff on the Issue - so the next session continues from it with a clean context.
disable-model-invocation: true
argument-hint: "[issue-number]"
---

# Hand off a long task

Issue: $ARGUMENTS. If that is empty, take the Issue number from the branch name (`feat/<issue#>-<slug>`). In Phases 3, 4 and 4b without an Issue, follow step 6 of `.claude/skills/phase/SKILL.md` instead.

1. **Check**: run the checks for what changed since the last handoff. Hand off only at a point where they pass. If one fails and you can't fix it now, say so in the handoff under "Open problems".
2. **Save the work**: Conventional Commit and push to the Issue's branch. Open a draft pull request with "Closes #<issue>" if there isn't one yet. Never push to main.
3. **Write the handoff**: add a comment to the Issue (`gh issue comment`, or the session's GitHub tools) that starts with `## Handoff` and has at most 10 lines:
   - **Done**: what works now, with the commit.
   - **Next**: the next step, specific enough to start without asking.
   - **Open problems**: failing checks, questions waiting on the user.
   - **Tried and dropped**: approaches that failed and why, so the next session doesn't repeat them.
   Write only what the next session can't find in the code, the Issue, the pull request or the ADRs. No file dumps or logs.
4. **Stop**: tell the user to type `/clear` (in the browser, start a new session), then `/new-feature <issue>`, which reads the latest handoff and continues from it.
