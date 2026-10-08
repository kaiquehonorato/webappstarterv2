# Phone Web App Starter Kit for Claude Code

A ready-to-copy kit for building a real phone app with Claude Code, published on Google Play and the App Store, together with the web side every store app needs: the backend it talks to, and the pages the stores require (privacy policy, support, account deletion). You decide, Claude plans with you, builds in small checked steps, explains what it did, and the project tests itself for bugs and security problems on every change. It follows Anthropic's official Claude Code guidance, OWASP (MASVS for the app, the Top 10 and ASVS for the backend), the OpenSSF guide for AI code assistants, WCAG 2.2, Apple's and Google's store rules, and Irish and EU law for a business established in Ireland (the GDPR with the Data Protection Act 2018, ePrivacy, consumer, accessibility, product security and AI rules). Sources are in `docs/REFERENCES.md`.

It is the sibling of the web app starter kit: the same phases, gates and rules, with everything that changes when the product is a binary installed on someone's phone instead of a page on your server.

## Is this the right kit for your project?

**It fits** an app people install on iPhone and Android phones, with accounts, a backend and real use cases: a client app for a business, booking, field service and inspections, delivery or order tracking, a community, an app for a company's own staff distributed privately. It fits best when the app needs what only an installed app does well (the camera, location, push notifications, use without a connection, a home-screen presence) and when the repository is empty, you are one or two people, and v1 takes weeks rather than days. If the business is in Ireland or the users are in the EU, the GDPR, ePrivacy, consumer, accessibility and Cyber Resilience Act rules in here are the reason to use it; users in other countries add their own, and Phase 7 checks them.

**It does not fit:**

| Project | Why, and what to do instead |
|---|---|
| A web app that people mostly open in a browser, even on phones | Store apps cost more: two platforms, store fees, a review before every release, and users who update weeks late. Use the web app starter kit; a well-built responsive web app covers most "app" ideas. Phase 0 asks this question first |
| A website wrapped in an app shell | Apple rejects apps that are mainly a repackaged website (App Review Guideline 4.2). Build the web app instead |
| A game | Games want an engine (Unity, Godot) and their own pipelines; the design system, rules and manual here assume an app made of screens and forms |
| Watch, TV, car or desktop apps | Not covered. The phone app can grow into them later, with their own phase |
| An existing app | The kit assumes an empty repository: Phase 2 picks the stack and Phase 3 builds the foundation. Take `AGENTS.md`, the rules in `.claude/rules/`, `docs/app-stores.md` and the phase 6, 7 and 9 prompts, and leave the rest |

**The small-app path.** If v1 is three screens or fewer, the full set of phases costs more than it returns. Run 0 (spec), 2 (stack, short), 3 (foundation) and 5 (features), then 4b (only the required pages), 6, 7 and 8 before the stores. In Phase 1, run only steps 1 and 2 (three visual directions and a clickable prototype on your phone). Skip Phase 4 and keep the tokens in one file until a second screen needs the same component. Everything else still applies: tests first, the authorization test per route, CI green before merge, and the store requirements in `docs/app-stores.md`, which don't shrink with the app.

## Start here
1. Copy this kit into an empty repository, `git init`, and push it to GitHub. (Easier: mark this repository as a template in its GitHub settings, then use "Use this template" for each new project.) Make sure Claude Code is 2.1.284 or later (`claude update`).
2. Read `docs/app-stores.md` once. Store accounts take days to weeks to verify, and a new personal Google Play account must run a 14-day closed test before it can publish, so some clocks start long before launch.
3. Open [`prompts/00-kickoff.md`](prompts/00-kickoff.md), fill in the lines about your idea, start `claude --model opus --effort high` in the repo, and paste the prompt.
4. Answer the interview. At the end of every phase Claude stops, shows you evidence, and tells you which prompt, model and effort to use next.

First time? [`docs/GETTING-STARTED.md`](docs/GETTING-STARTED.md) walks through every step, from installing Claude Code and the phone tooling to an approved spec.

Prefer a visual guide? Open [`docs/walkthrough.html`](docs/walkthrough.html) in a browser (download it or clone the repo first; GitHub shows HTML files as source). It covers the same steps with a terminal/browser switch, a kickoff prompt builder and a phase-by-phase map.

## The phases

| # | Phase | What you get | Model · effort | Prompt |
|---|---|---|---|---|
| 0 | Discovery | Your idea questioned (including whether it needs the stores at all), platforms and distribution chosen, a v1 scope you chose, SPEC.md, glossary, roadmap | Opus · high | `prompts/00-kickoff.md` |
| 1 | Prototype and manual | 3 visual directions, a clickable prototype you open on your own phone, an illustrated use-case manual with screenshots | Sonnet · medium | `prompts/01-prototype-manual.md` |
| 2 | Architecture and stack | App framework, backend, hosting, database, auth, builds and update strategy chosen with you, with costs; data model; threat model; GitHub Issues; a table of anything the documents disagree on | opusplan · high, plan mode | `prompts/02-architecture.md` |
| 3 | Foundation | Monorepo, CI, tests on both platforms, layered security pipeline, signing kept out of reach, build profiles, forced-update check, Claude Code hooks | Sonnet · medium | `prompts/03-foundation.md` |
| 4 | Design system | Tokens that follow the phone's text size and dark mode, components with every state, platform conventions, gestures, app icon and splash screen | Sonnet · medium | `prompts/04-design-system.md` |
| 4b | Store pages and public site | The pages both stores require (privacy policy, support, account deletion), the files that make app links work, and a landing page only if you want one, with indexing and analytics only if you ask for them | Sonnet · medium | `prompts/04b-public-site.md` |
| 5+ | Features | One Issue per session, tests first, checked on iOS and Android, reviewed, manual updated | Sonnet · medium | `prompts/05-feature.md` or `/new-feature <issue>` |
| Every 3–5 features | Security audit | Automated scans, including a scan of the built app; 103 manual checks mapped to OWASP Top 10:2025 and MASVS, covering the common 50-point checklist; then Cloudflare's vulnerability hunt for what no checklist lists | Opus · high | `prompts/06-security-audit.md` |
| Before launch | Legal, privacy and store policy review | Risk report, the answers for Apple's privacy labels and Google's Data safety form, and drafts for a lawyer | Opus · high | `prompts/07-legal-review.md` |
| Launch | Store submission and operate | Readiness check, your launch checklist (`docs/launch-checklist.md`), store listing, review notes, phased rollout, runbook, final manual, scheduled checks | Sonnet · high | `prompts/08-launch.md` |
| Every release | Store release | Version numbers, a store build or an over-the-air update, test builds on real devices, submission after your OK, staged rollout | Sonnet · medium | `/store-release <version>` |
| Every 5–10 features | Cleanup review | Dead code, unused permissions and SDKs, duplicates and needless complexity found with evidence, then removed in small PRs | Opus · high, then Sonnet · medium for the removals | `prompts/09-cleanup-review.md` |

Each row is a starting point. When Claude skips work, raise the effort; when it tries hard and is still wrong, move to Opus. `docs/models-and-tokens.md` says when to use each level from medium to max.

Why separate sessions: Claude's quality drops as its context fills up. A fresh session per phase (per step in Phases 3, 4 and 4b) with a precise prompt is both better and cheaper; the plan survives in files (`SPEC.md`, `docs/ROADMAP.md`, ADRs), not in chat history.

Where sessions run: anything that needs an iOS simulator, an Android emulator or a real phone runs on your computer (the terminal, or a Local session in the desktop app) or in CI. A browser session at claude.ai/code runs on a Linux machine in the cloud: it can write code and run the backend and unit tests, but it can't open a simulator, so Claude says which device checks it could not run.

## What's inside

```text
phone-web-app-starter-kit/
├── AGENTS.md                      ← rules every agent follows (short on purpose)
├── CLAUDE.md                      ← imports AGENTS.md
├── SECURITY.md                    ← vulnerability reporting, breach plan (GDPR / DPC, Cyber Resilience Act), forced updates
├── .env.example                   ← placeholders only; marks which values ship inside the app
├── .gitignore                     ← keeps .env files, signing keys and app builds out of git
├── .github/                       ← pull request template with a definition of done; Issue templates
├── .claude/
│   ├── settings.json              ← blocks secrets and signing keys; asks before store, destructive or production commands
│   ├── rules/                     ← loaded only when Claude reads matching files
│   │   ├── mobile.md              (the app: screens, device storage, permissions, links)
│   │   ├── backend.md             (the API the app calls)
│   │   └── web.md                 (the store pages and any web version)
│   ├── agents/                    ← subagents, each with its own model
│   │   ├── explorer.md            (Haiku: cheap codebase search)
│   │   ├── test-writer.md         (Sonnet: tests first)
│   │   ├── ui-reviewer.md         (Sonnet: states, accessibility, platform conventions, copy, motion)
│   │   ├── security-reviewer.md   (Opus, high effort: backend and app security, GDPR, licenses)
│   │   └── verifier.md            (Sonnet, high effort: tries to prove the task is not done)
│   └── skills/                    ← work in the terminal and in browser sessions, nothing to install
│       ├── new-feature/           ← /new-feature <issue>: Issue → tests → code → review → PR
│       ├── store-release/         ← /store-release <version>: build, test, submit, roll out
│       ├── use-case-manual/       ← /use-case-manual: screenshots manual from the prototype or the app
│       ├── phase/                 ← /phase <n>: starts a phase, or the next step of Phases 3, 4 and 4b
│       ├── ask/                   ← /ask <question>: answers from the kit's files on Haiku, in its own context
│       ├── frontend-design/       ← Anthropic's design skill (Apache-2.0)
│       ├── security-audit/        ← Cloudflare's vulnerability hunt (MIT)
│       ├── systematic-debugging/  ← from Superpowers (MIT): root cause before any fix
│       └── verification-before-completion/ ← from Superpowers (MIT): no "done" without proof
├── prompts/                       ← one per phase; start one with /phase <n>, or paste it into a fresh session
│   └── 00-kickoff.md … 09-cleanup-review.md
└── docs/
    ├── GETTING-STARTED.md         ← step by step, from installing Claude Code to an approved spec
    ├── app-stores.md              ← accounts, costs, distribution options, review rules and yearly deadlines
    ├── ROADMAP.md                 ← where the project is; updated at every gate
    ├── launch-checklist.md        ← what only you can do: store accounts, signing keys, two-step sign-in, backups
    ├── GLOSSARY.md                ← one word, one meaning, in code and UI
    ├── vibe-coding-mistakes.md    ← the 52 common mistakes and the guardrail for each
    ├── models-and-tokens.md       ← which model and effort when, how to switch, how to spend less
    ├── skills-and-plugins.md      ← built-in commands, account skills, official plugins worth installing
    ├── security-model.md          ← what "secure" means for an app on someone else's phone
    ├── writing-style.md           ← UI text, permission prompts, notifications and store copy
    ├── plugin-safety-checklist.md ← vet any plugin, MCP server or skill before installing
    ├── assets-licenses.md         ← record of font, icon, image, sound and store-asset licenses
    ├── REFERENCES.md              ← the sources behind every rule
    └── adr/0000-template.md       ← architecture decision record template
```

## Golden rules
1. **You decide, Claude proposes.** Platforms, stack, hosting, database, store accounts, money, data and anything legal need your explicit OK. Nothing is submitted to a store or pushed to users without it.
2. **One session = one phase, one step (Phases 3, 4 and 4b) or one Issue.** `/clear` between unrelated tasks (in the browser, a new session).
3. **Explore → plan → code → verify.** Skip the plan only if the change fits in one sentence.
4. **Every claim needs evidence:** a command and its output, a test, or a screenshot from both platforms. Nothing is called "secure" without the test that shows it.
5. **The app is public.** Anyone can download it, unpack it and call your API without it. Secrets and rules live on the server; the app only asks.
6. **Fresh eyes review.** A subagent or new session reviews better than the one that wrote the code.
7. **After two failed corrections, start fresh** with a better prompt.
8. **Right model, right effort:** Sonnet builds, Opus decides and audits, Haiku searches. Raise effort when work was skipped; switch to Opus when Claude tried and was still wrong (`docs/models-and-tokens.md`).
9. **Few, vetted plugins:** the skills the kit needs ship in `.claude/skills/` and work in the browser too; add a plugin only when a phase needs it (`docs/skills-and-plugins.md`, `docs/plugin-safety-checklist.md`).
10. **Delete what doesn't earn its place.** A cleanup review every 5–10 features keeps the codebase and the app small (`prompts/09-cleanup-review.md`).
11. **AI helps with compliance; it does not replace a lawyer.**
