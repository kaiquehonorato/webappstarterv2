# Skills and plugins worth using

Few, official, and only for the current phase. Every plugin runs with your permissions and adds to Claude's context, so each extra one costs tokens and widens the attack surface. The skills the kit relies on are files in `.claude/skills/`, so they work in every session, browser included. Vet anything not listed here with docs/plugin-safety-checklist.md.

## Built into Claude Code (nothing to install)

| Command | What it does | Where this kit uses it |
|---|---|---|
| `/design` | Drafts UI mockups as artboards you can edit and export as PNG or PDF (needs the Anthropic API and artifacts) | Phase 1, optional |
| `/design-sync` | Uploads your React design system so `/design` mockups use your real components. Check first that it handles your app framework's components | After Phase 4, optional |
| `/run`, `/verify`, `/run-skill-generator` | Launch the API and the app (on a simulator, on your computer) and confirm a change works in it, not only in tests. Claude uses `/run` on its own; only you can start `/verify` | Phase 3 onwards |
| `/code-review [effort] [--fix]` | Bug-focused review of the current diff in a fresh subagent. `ultra` runs a deeper cloud review (free runs, then usage credits) | Every PR (step 7 of `/new-feature`) |
| `/security-review` | Security pass over the branch's diff | PRs touching auth, data, APIs, uploads, permissions, deep links |
| `/simplify` | Cleanup: reuse, simplification, efficiency (it doesn't look for bugs) | After a feature works |
| `/deep-research <question>` | Web research with cross-checked, cited sources. Only you can start it | Phases 0 and 2, for facts such as pricing |
| `/goal <condition>` | Keeps working until a checkable condition holds, judged by a separate model | Long migrations and refactors |
| `/advisor opus` | Sonnet works, Opus advises at key decisions (experimental) | Phase 5 |
| `/schedule` | Cloud routines for recurring jobs, such as a monthly security scan or the yearly store deadlines | Phase 8 |
| `/context`, `/usage`, `/doctor`, `/skill-doctor`, `/insights` | Show what fills the context, what it costs, and which skills or plugins you never use | Any time |

## Skills that ship with the kit
They live in `.claude/skills/`, so they load in the terminal, the desktop app and browser sessions alike, with nothing to install. Until a task needs one, Claude sees only its one-line description: under 300 tokens for the four copies together.

| Skill | What it does | Source and license |
|---|---|---|
| `frontend-design` | A visual direction grounded in the product's subject, deliberate typography, one memorable element, copy that helps; accessible, reduced motion respected. Written with web pages in mind: in this kit it sets the app's look in Phases 1 and 4 and builds the store pages in 4b, while the platforms' conventions decide where things go on native screens | Anthropic, Apache-2.0. Unchanged copy from [anthropics/skills](https://github.com/anthropics/skills) (commit `8a1541c`), the same file as the `frontend-design` plugin |
| `systematic-debugging` | Root cause before any fix; after three failed fixes, question the design instead of trying a fourth | [Superpowers](https://github.com/obra/superpowers) 6.4.2 (commit `8ca22db`) by Jesse Vincent, MIT |
| `verification-before-completion` | No "done", "fixed" or "passing" without a command run in this session and its output | Superpowers 6.4.2, MIT |
| `security-audit` | A six-phase vulnerability hunt: map the trust boundaries, send an isolated hunter at each, then give every candidate to a fresh agent that tries to disprove it. Severity only on confirmed findings | Cloudflare, MIT. Unchanged copy from [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) (commit `c1c8a8c`), minus its two test files |
| `new-feature` | `/new-feature <issue>`: Issue → tests → code → checks on iOS and Android → review → PR | This kit |
| `store-release` | `/store-release <version>`: store build or over-the-air update, test builds on real phones, store forms, submission after your OK, staged rollout | This kit |
| `use-case-manual` | `/use-case-manual`: regenerates the illustrated manual from the prototype or the app on iOS and Android | This kit |
| `phase` | `/phase <n>`: starts a phase from its prompt, or the next step of Phases 3, 4 and 4b from the handoff in docs/ROADMAP.md | This kit |
| `ask` | `/ask <question>`: answers from the kit's files on Haiku, in its own context, so only the answer enters the conversation | This kit |

**Why Cloudflare's audit skill is a copy too.** Its own instructions install it with `npx skills add`, which fetches and runs the newest Skills CLI each time, the reason this kit skips the Playwright and Chrome plugins as well. A copy is pinned to a commit you read, and it works in browser sessions. It is not in the Anthropic Directory, so there is no publisher tier to read: what stands behind it is the `cloudflare` GitHub organization, an MIT license in Cloudflare's name, and the blog post describing the harness it grew into. What it contains: 15 instruction files, a JSON schema, and two validators whose only imports are `node:fs`, `node:path` and `node:util`. They read one JSON file, print errors and exit. No network, no child process, no hooks, no MCP server and no install step, which is `contained` reach in the directory's vocabulary. It refuses to run your code at all unless an operating-system sandbox with no network and hard limits is available, and records the missing control instead.

**Why copies instead of plugins.** A browser session at claude.ai/code doesn't install the plugins a repository turns on, so a plugin would leave those sessions without the skill. Copies also cost less: the Superpowers plugin adds about 1,400 tokens to every session.

**Why only two Superpowers skills.** Root-cause debugging and checking before "done" turn two one-line rules in AGENTS.md into step-by-step habits. The rest repeats what the kit already does: `test-driven-development` duplicates the test-writer subagent and the tests-first rule in AGENTS.md, and `writing-plans` duplicates the plan step of `/new-feature`. Every feature touches several files, so `writing-plans` would run on almost every one, adding a second plan file and a second approval stop. Brainstorming, subagent-driven development, parallel agents and worktrees repeat the phase prompts and `/new-feature` at 4,000 to 8,000 tokens per use.

**What changed in the copies.** `frontend-design`, `verification-before-completion` and `security-audit` are unchanged, except that `security-audit` leaves out `validate-findings.test.cjs` and `validate-coverage-ledger.test.cjs`, which test its two validators and are the only files in it that start a child process. Both suites were run from a fresh clone of commit `c1c8a8c` on Node 22 before copying and passed with no failures; run them there again when you update the copy, rather than keeping them in the repo. In `systematic-debugging`: failing tests come from the kit's test-writer subagent, the other reference uses the skill's name in this repo instead of a `superpowers:` name, the debugging example no longer prints a secret's value to the log, and files used only to develop the skill (a TypeScript example, test scenarios, a creation log) were left out. Each folder keeps its license file.

**Updating a copy.** Read the new version in full first (docs/plugin-safety-checklist.md), copy it over, apply the same changes again, and update the version and commit in the table above.

**Coming from an earlier version of this kit?** If you installed the Superpowers or `frontend-design` plugin for it, turn the plugin off with `/plugin` so the same skills don't load twice.

## Plugins: terminal and desktop only
Plugins install per computer, in the terminal or a Local session of the desktop app. Browser sessions don't load them, so nothing in the kit depends on one. Install with `/plugin install <name>@claude-plugins-official`, then `/reload-plugins`.

**Maintained by Anthropic, worth installing when the phase comes**

Every row below was checked in the directory: `publisher.tier` is `anthropic` and `upstream.repo_url` is `anthropics/claude-plugins-official` or `anthropics/knowledge-work-plugins`. Reach is from the same listing (docs/plugin-safety-checklist.md explains it).

| Plugin | Reach | What you get | When |
|---|---|---|---|
| `claude-security` | privileged | A multi-agent vulnerability scan of the repo or a diff. Each finding is checked by independent verifiers before it reaches the report; it can draft patch files | Phase 6, and before each release |
| `Design` | remote | Seven design skills, of which three fit this kit: `design-critique` (the second opinion on the look that nothing else here gives), `accessibility-review` and `ux-copy`. It bundles nine MCP servers (Figma, Notion, Slack, Asana, Atlassian, Gmail, Calendar, Intercom, Linear), which is why its reach is `remote`. **Connect none of them**; the three skills work without any | Phases 1 and 4 |
| `playground` | contained | A one-page tool with sliders for spacing, color, fonts and animation timing. You tune by eye, then copy one prompt back to Claude: cheaper than ten rounds of "a bit rounder" | Phases 1, 4 and 4b, optional |
| `security-guidance` | privileged | Warnings on dangerous patterns as Claude edits, a review of each turn's diff, and a deeper review at each commit that traces data across files. It costs the most of any plugin here: every turn's changes go to an extra Opus call. Setting `ENABLE_STOP_REVIEW=0` keeps only the commit review. Needs Python 3.8 or later | Optional, Phase 3 onwards |
| `claude-md-management` | privileged | Audits and trims CLAUDE.md and AGENTS.md, captures lessons from sessions | Monthly |
| `skill-creator` | contained | Build and test your own skills, for example one built from the Three.js docs if 3D is central to your product | When a workflow repeats |

**An LSP for your language** gives precise symbol lookup and type errors right after edits, so fewer files get read. Worth having from Phase 3, but check the listing before you install: the TypeScript entries in the directory are `community` tier with `privileged` reach and no upstream repo, which the safety checklist rules out. Prefer your editor's own language server, or read the plugin's hooks first.

**Your stack's vendor plugin**, only after Phase 2 picks the stack, and read what it runs before installing. That includes the app framework: if Expo, React Native or Flutter has an entry in the directory, check its tier and reach like any other, and don't assume one exists or that the name proves who published it. Tiers differ by vendor, so check each one rather than assuming the brand name means the vendor published it:

| Entry | Tier | Note |
|---|---|---|
| `stripe` | partner | `github.com/stripe/ai` |
| `sentry` | partner | `github.com/getsentry/plugin-claude` |
| `neon` | partner | `github.com/neondatabase/agent-skills` |
| `supabase` | partner | Published by `supabase-community`, not by the Supabase organization |
| `Netlify` | community | Authored by Netlify, but at community tier with no upstream repo listed. Read it before installing, or use the Netlify CLI instead |

**Not recommended, and why**

| Plugin | Why not |
|---|---|
| `superpowers`, `frontend-design` | Their useful skills already ship in `.claude/skills/`; installing them loads the same skills twice |
| `code-review`, `pr-review-toolkit` | The built-in `/code-review` and the kit's reviewer subagents do the same job |
| `playwright` (Microsoft) | It downloads and runs the newest npm version each time, not a reviewed one. The Playwright test runner in the kit takes the same screenshots, with a pinned version and no cost per turn |
| `modern-web-guidance` (Google Chrome) | It also runs the newest version each time, and calls itself mandatory for all HTML, CSS and JavaScript work, so it would run on most frontend turns |
| `context7` (Upstash) | Your questions go to a hosted service |
| `figma` | It needs a paid Figma seat and lists 14 skills in every session |
| `chrome-devtools-mcp` | About 60 tools added to every turn, and usage statistics sent to Google by default |

## "Audited by Anthropic"? No. Read the tier instead
The catalog is now one directory, the Anthropic Directory, which absorbed the separate marketplaces that came before it. Each entry states who published it:

| `publisher.tier` | Who | What it means for you |
|---|---|---|
| `anthropic` | Anthropic's own work, upstream in `anthropics/*` | The only tier this kit installs at `privileged` reach |
| `partner` | The vendor itself, with `publisher.name` naming the org and usually its repo listed | Trust it as far as you trust that company with your code |
| `community` | Anyone else. The author name shown is a claim, not a verified identity | Read all of it first. A company's name in the title does not make it that company's plugin |

Every tier carries the same `basis: directory_review`, so **no tier is a promise that anyone audited the code**, and being listed is not an endorsement. Read each entry's `reach` and tier (docs/plugin-safety-checklist.md), read the hooks and MCP servers of anything that is not `anthropic` tier, and keep Claude Code updated (the kit needs 2.1.284 or later, which includes the Plugin4Shell fix from 2.1.179).

The tables above were checked against the directory on 6 October 2026. Tiers, reach and names change, so read the listing itself when you install, not this file.

## Anthropic skills on your account
These come with your Claude account instead of as plugins, so they load in browser sessions too. Each costs only its one-line description until something triggers it. The ones worth knowing for this kit:

| Skill | Use it for | When |
|---|---|---|
| `dataviz` | Any chart, dashboard, stat tile or sparkline in the product: one visual system with accessible colors in light and dark, instead of a chart library's defaults | Phase 4 onwards, if the product shows numbers |
| `session-start-hook` | A SessionStart hook so a cloud or browser session installs dependencies and runs the tests without being told how | Phase 3, Step 5 |
| `fewer-permission-prompts` | Builds an allowlist in `.claude/settings.json` from the commands you keep approving | Phase 3, once the commands settle |
| `update-config` | Edits `.claude/settings.json` correctly: permissions, env vars, hooks | Any time the settings change |
| `pdf`, `xlsx`, `docx` | Only if the product itself generates documents (invoices, reports, exports) | Phase 5, inside that feature |
| `claude-api` | Only if the product calls the Claude API. Never answer a model, pricing or limit question from memory | Phase 2 onwards, if AI is a feature |

Skip these, with the reason:

| Skill | Why not |
|---|---|
| `theme-factory` | Preset themes are the templated look Phase 1 exists to avoid. Your tokens come from the direction you chose |
| `web-artifacts-builder`, `artifact-*` | For claude.ai artifacts, not for your product's code |
| `canvas-design`, `algorithmic-art` | Posters and generative art, not product UI |
| `init` | AGENTS.md and CLAUDE.md already do this job for this kit |
| `docs`, `doc-coauthoring` | Phase 0 writes SPEC.md into the repo, where the code and the tests can point at it |

## What this kit adds
- Skills in .claude/skills/: the four copies above, plus `/new-feature <issue>`, `/store-release <version>`, `/use-case-manual`, `/phase <n>` and `/ask <question>`.
- Subagents in .claude/agents/: explorer, test-writer, ui-reviewer, security-reviewer, verifier.
- Rules in .claude/rules/: mobile (the app), backend (the API) and web (the store pages), loaded only when Claude reads matching files.
- Pull request and Issue templates in .github/, with a definition of done.
