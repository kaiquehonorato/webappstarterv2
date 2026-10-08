---
name: phase
description: Start a phase from its prompt in prompts/, or the next step of Phases 3, 4 and 4b, without pasting the prompt.
disable-model-invocation: true
argument-hint: "[phase: 1, 2, 3, 4, 4b, 6, 7, 8 or 9]"
---

# Start a phase, or its next step

Phase: $ARGUMENTS. If that is empty, take the first phase in docs/ROADMAP.md that isn't approved.

1. **Find the prompt**: the file in prompts/ whose name starts with the phase number (`04b` for 4b). Phase 0 needs the user's idea, so ask them to paste the filled-in prompt from prompts/00-kickoff.md instead. Phase 5 goes one Issue at a time: point them to `/new-feature <issue>`.
2. **Check the model**: if this session's model differs from the prompt's "Model:" line, say so in one line with the commands to switch (`/model`, `/effort`), and ask whether to switch before starting. Switching later throws away the cache.
3. **Find the step**: in Phases 3, 4 and 4b, read "Current step" in docs/ROADMAP.md. If it names this phase, start at that step and read its handoff; otherwise start at Step 1.
4. **Start from main** (Phases 3, 4 and 4b): if the previous step's pull request is still open, ask the user before starting. Otherwise switch to main and pull.
5. **Follow the prompt**: read the prompt file and follow the instructions in its `text` block as if the user had pasted them. In Phases 3, 4 and 4b, do only the current step.
6. **End of a step** (Phases 3, 4 and 4b): in the step's pull request, rewrite "Current step" in docs/ROADMAP.md with the next step and a handoff of at most three lines, and put any decision under Decisions. Show the evidence, stop, and tell the user to merge the pull request, type `/clear`, then `/phase <n>`. After the last step, set "Next" to none and follow the prompt's gate.
