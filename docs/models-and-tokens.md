# Models, effort levels and tokens

How to pick the model and effort level for each phase, how to switch, and how to spend fewer tokens without losing quality. Facts as of October 2026; run `/model` to see what your account offers today.

## The models
Aliases always point to the newest recommended version, so "always use the newest model" just means using the alias.

| Alias | Resolves to (Anthropic API, Oct 2026) | Use it for |
|---|---|---|
| `sonnet` | Sonnet 5.5 | Default for building: features, tests, refactors |
| `opus` | Opus 5.5 | Discovery, architecture, security reviews, hard bugs |
| `haiku` | Haiku 5.5 (Haiku 4.5 before Claude Code 2.1.293) | Search subagents, `/ask`, simple edits |
| `fable` | Fable 5.1 | The hardest and longest autonomous work. On some plans it bills usage credits instead of your plan's limits |
| `best` | Fable 5.1 where available, otherwise Opus 5.5 | When you want the strongest model without thinking about it |
| `opusplan` | Opus in plan mode, Sonnet while executing | Planning-heavy sessions such as Phase 2 |

Use aliases in Claude Code. Pin full model IDs (for example `claude-opus-5-5`) only in your product's own code that calls the Claude API, and upgrade those on purpose after testing.

## Effort levels
Effort controls how much Claude does before it checks back with you: how many files it reads, how many tools it runs, how many steps it takes and how much it thinks between them. Thinking is always on for Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable; effort is the dial. Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1 offer all five levels; Opus 4.6 and Sonnet 4.6 have no `xhigh`; Haiku 4.5 has no effort setting.

| Level | Use it for |
|---|---|
| `low` | Quick exchanges you review each time: renames, small edits, first sketches |
| `medium` | The default on Opus 5.5 and Sonnet 5.5. Day-to-day work with a clear scope |
| `high` | Work where verification and edge cases matter: bug fixes, migrations, reviews |
| `xhigh` | Deeper reasoning when `high` still misses things |
| `max` | Hard problems you want worked through without you, such as a security scan. It can overthink, so never as a default |

Two extras:
- Type `ultrathink` anywhere in a message for deeper reasoning on that turn only.
- `ultracode` (a setting, `/effort ultracode`) makes Claude plan a multi-agent workflow for each substantial task. Powerful and token-hungry; use it for big audits or migrations, not daily work.

Effort names are calibrated per model: in Anthropic's testing, Opus 5.5 at `medium` matched or beat Opus 5 at `high`. Moving up from an older model, start at `medium` instead of carrying over your old level.

## Opus or Sonnet, medium to max
When Claude gets something wrong, ask Anthropic's question: did it not try hard enough, or did it not know enough?
- **It didn't try hard enough**: it skipped a file, didn't run the tests, stopped early or didn't double-check. Raise the effort one step and keep the model.
- **It didn't know enough**: it had the context, clearly tried, and was still wrong; or the problem is a subtle bug, an unfamiliar domain or an architecture decision. Switch to Opus. Sonnet at `max` doesn't replace knowledge it lacks.
- **The work became routine again**: step back down. Sonnet at `medium` saves real money at no quality cost.

Start at the first row that fits and move one row at a time.

| Model · effort | Use it for | In this kit |
|---|---|---|
| Sonnet · medium | Changes you can describe precisely: features with clear acceptance criteria, UI from the design system, setup work, routine releases. The default for building | Phases 1, 3 and 4, most features, `/store-release`, the removal PRs after a cleanup review |
| Sonnet · high | Work where checking matters: bug fixes, migrations, refactors across several files, long checklists | Bug fixes in Phase 5, Phase 8, the verifier subagent |
| Sonnet · xhigh | A long job with a written plan that you want finished in one pass, such as tests for an untested module or one mechanical change across many files | Large refactors after an approved plan |
| Sonnet · max | Rarely. A mechanical job left running without you, where finishing matters more than cost. If the job needs judgment, use Opus instead | `/goal` runs on a clear checklist |
| Opus · medium | Judgment on a small scope: a design question, reviewing a plan, explaining unfamiliar code, a tricky bug in one place | A decision in the middle of a Sonnet project; or keep Sonnet and run `/advisor opus` |
| Opus · high | Decisions that are expensive to undo, and reviews where a miss is costly | Phases 0, 2, 6 and 7, the cleanup review, the security-reviewer subagent |
| Opus · xhigh | A bug that survived two attempts, money or permission logic, race conditions, data migrations, a domain new to you, an architecture with many constraints | Raising a phase that didn't convince you (table below) |
| Opus · max | Hard problems you want worked through without you. Returns diminish and it can overthink, so test it before relying on it | The deep scan in Phase 6, a final audit before launch |
| Fable (or `best`) | The hardest and longest autonomous work, if your plan has it. It can bill usage credits | Instead of Opus · max |

Cost: each step up spends more tokens, and Opus costs more per token than Sonnet. On hard work a bigger model can still cost less in total, because it needs fewer attempts. `max` applies to one session only and can't be saved as a default; start that session with `--effort max`.

## Per phase

| Phase | Start with | Raise to | When |
|---|---|---|---|
| 0 Discovery | `opus` · high | `opus` · xhigh | A regulated domain (health, money, children), or the interview keeps missing obvious risks |
| 1 Prototype and manual | `sonnet` · medium | `opus` · medium | The three visual directions look generic or alike |
| 2 Architecture | `opusplan` · high | `opus` · xhigh | Many constraints (compliance, data location, a tight budget), or the options don't convince you |
| 3 Foundation | `sonnet` · medium | `sonnet` · high | CI keeps failing on setup details |
| 4 Design system | `sonnet` · medium | `sonnet` · high | Components miss states or accessibility checks |
| 5 Features | `sonnet` · medium | `sonnet` · high for bugs; `opus` · high or xhigh | Opus for a bug that survived two attempts, and for money, permission or concurrency logic |
| 6 Security audit | `opus` · high | `opus` · max, or `fable` | The deep scan before launch and after changes to sign-in, payments or data access |
| 7 Legal review | `opus` · high | `opus` · xhigh | Children, health data, AI features, or personal data leaving the EEA |
| 8 Launch | `sonnet` · high | `opus` · high | A store rejection you don't understand, a failed restore test or a production incident |
| Each release (`/store-release`) | `sonnet` · medium | `sonnet` · high; `opus` · high | High for the first release and any release with new permissions, SDKs or data; Opus for a rejection or a rollout that had to be paused |
| 9 Cleanup review | `opus` · high | `opus` · xhigh | A large or old codebase with many dynamic references. The removal PRs run on `sonnet` · medium |
| Subagents | explorer `haiku` · test-writer, ui-reviewer `sonnet` · verifier `sonnet`/high · security-reviewer `opus`/high | | Set in .claude/agents/ |

## How to switch
- For one session, without changing your defaults (the recommended way, since each phase starts a new session): `claude --model sonnet --effort medium`
- Inside a session: `/model opus` and `/effort high` switch and also save the choice as your default for new sessions. To change only the current session, open the `/model` or `/effort` picker and press `s` instead of Enter. `/effort auto` returns to the model's default; `max` always applies to the current session only. In the `/model` picker, the arrow keys move the effort slider.
- A default effort per model, in `~/.claude/settings.json`:
  ```json
  { "modelSettings": { "claude-opus-5-5": { "effort": "high" }, "claude-sonnet-5-5": { "effort": "medium" } } }
  ```
- Subagents and skills: `model:` and `effort:` in their frontmatter (see .claude/agents/).
- The advisor (experimental, Anthropic API only): keep Sonnet as the main model and run `/advisor opus`. Sonnet does the work and consults Opus before big decisions, when an error repeats, and before calling a task done. Opus quality at the moments that matter, Sonnet prices for the rest.

## What switching costs
Claude Code caches the conversation so each turn doesn't pay for the whole history again. Some actions throw that cache away and make the next turn slow and expensive:
- **Changing model mid-session.** Each model has its own cache. Pick the model when the session starts.
- **`opusplan`**: every switch into or out of plan mode is a model switch. Fine for "plan once, then build"; wasteful if you toggle often.
- **A skill whose frontmatter names another model** switches model for that turn.
- **Effort changes** keep the cache on Opus 5.5, Sonnet 5.5 and Fable 5.1 (with an API key or subscription); on older models they don't.
- **Long breaks.** The cache lives about 1 hour on a subscription (5 minutes by default on API keys and usage credits). After a longer break, resume from a summary or start fresh with `/clear`.

## Where the tokens go
In one long session of this kit (65 calls to Opus 5.5, at October 2026 API prices), about 48% of the cost was new content entering the conversation, 26% was Claude's replies and thinking, and 25% was re-reading the conversation on each call. So:
- Everything that enters the conversation (a file, a command's output, a reply) is paid for when it arrives, then again on every later call while it stays, at a twentieth of the input price on Opus 5.5 and Sonnet 5.5.
- Replies and thinking cost the most per token: output costs five times as much as input.
- In a chat, the model and the effort level matter most. In a build session with hundreds of tool calls, what enters the context and how long the session runs matter most.

## Token-saving checklist, biggest wins first
1. One task per session. `/clear` between unrelated tasks (`/rename` first, to find it again with `/resume`).
2. A fresh session per phase, and per step in Phases 3, 4 and 4b: merge the step's PR, `/clear`, then `/phase <n>`. The plan lives in files (SPEC.md, docs/ROADMAP.md, ADRs), not in chat history.
3. Sonnet by default, Opus or Fable where judgment matters, Haiku for search subagents.
4. Default effort; raise it only when work was skipped. Never `max` by default.
5. Specific prompts: point at files with `@`, state the outcome and the check.
6. Plan mode for multi-file changes. A wrong direction is the most expensive mistake.
7. Subagents for exploration and noisy output (test runs, build logs, documentation lookups). Native build output (Xcode, Gradle) is long: keep only the errors.
8. Open screenshots only when needed: about 1,400 tokens for a 1280×800 image and 450 for 390×844. A phone screenshot at full resolution costs three or more times as much as the same one resized to 390 px wide, so resize device screenshots before opening them. Look at the screens you changed, on one platform unless the other differs, not the whole manual.
9. Quiet output: the Stop hook returns only failures, and `pnpm check`, the test reporters, Gradle and Xcode print one line on success and only the failures otherwise (Phase 3, Step 5). For a failed CI job, read only the failing step's last 100 lines first.
10. A short AGENTS.md; area rules in `.claude/rules/` with `paths`; workflows in skills, which load only when used.
11. CLI tools (`gh`, the cloud CLIs) over MCP servers. Disable servers and plugins you don't use (`/mcp`, `/plugin`); `/skill-doctor` and `/doctor` find the unused ones.
12. A code intelligence plugin for your language (e.g. typescript-lsp) replaces grep-and-read loops with precise lookups. Phase 3, Step 5 enables it.
13. `/btw` for side questions: the answer never enters the conversation. `/ask <question>` for questions about the kit: Haiku reads the docs in its own context and only the answer comes back. For a session of questions, `/output-style concise` gives the result first and no recaps; building keeps the default style, so the evidence at each gate stays complete.
14. `/compact <what to keep>` when you must continue; `/clear` when you don't. `.claude/settings.json` sets `autoCompactWindow` to 200,000 tokens, so on a model with a 1M-token window a session compacts at about 200,000 tokens instead of close to 1M. For one session that needs more, start it with `CLAUDE_CODE_AUTO_COMPACT_WINDOW=500000 claude`.
15. When Claude takes a wrong turn, `/rewind` (Esc Esc) to before it and say what you want instead: a correction on top keeps the wrong attempt in the context. After two failed corrections, restart with a better prompt instead of a third correction.
16. Agent teams can use about 7 times the tokens of a normal session. Only for truly parallel work.
17. `/fast` buys speed with money; it doesn't save tokens.
18. Measure: `/context` (what fills the window), `/usage` (cost and cache hit rate), `/insights`, the session-report plugin, or a status line that shows context use and cost (`/statusline` writes one).

## Cheap and reliable at the same time
The goal is the lowest cost per correct feature, not per message. The checks in this kit (tests first, the Stop hook, reviewer subagents, CI) are what make the cheaper model safe to use: when Sonnet slips, a check catches it before you do.

Sources: Claude Code docs on model configuration, costs, prompt caching, the advisor tool, skills, the context window, output styles and code intelligence; Anthropic's pricing page; Anthropic's blog post "Choosing a Claude model and effort level in Claude Code" (links in docs/REFERENCES.md).
