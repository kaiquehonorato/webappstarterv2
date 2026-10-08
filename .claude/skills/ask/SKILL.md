---
name: ask
description: Answer a question about this kit or project from the repository's own files, on Haiku in a separate context, so only the answer enters the conversation.
disable-model-invocation: true
argument-hint: "[question]"
context: fork
agent: explorer
---

Answer this question from the files in this repository: $ARGUMENTS

1. Search before reading: Grep for the question's key words in AGENTS.md, SPEC.md, docs/, prompts/ and .claude/ (leave out .claude/skills/security-audit/ unless the question is about it). Read only the sections that answer it.
2. Answer in plain language in at most 10 lines, with the file and line for each fact, e.g. `docs/ROADMAP.md:12`.
3. If the files don't answer it, or the answer needs a judgment call, say so in one line and suggest asking in the main session. Don't guess.
