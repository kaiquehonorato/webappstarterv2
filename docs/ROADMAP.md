# Roadmap

Where the project stands. Claude updates this file at every gate; you approve the changes in the PR.

| # | Phase | Status | Gate evidence | Start the session with | Done on |
|---|---|---|---|---|---|
| 0 | Discovery | not started | SPEC.md, GLOSSARY.md approved; platforms and distribution chosen | `claude --model opus --effort high`, then paste the filled-in kickoff prompt | |
| 1 | Prototype and use-case manual | not started | direction chosen, prototype tried on your phone, docs/manual/ approved | `claude --model sonnet --effort medium`, then `/phase 1` | |
| 2 | Architecture and stack | not started | ADRs, docs/architecture.md, Issues | `claude --model opusplan --effort high --permission-mode plan`, then `/phase 2` | |
| 3 | Foundation | not started | CI green with every gate; a preview build installed on an iPhone and an Android phone | `claude --model sonnet --effort medium`, then `/phase 3` (one session per step) | |
| 4 | Design system | not started | component catalog approved on both platforms; app icon and splash screen | `claude --model sonnet --effort medium`, then `/phase 4` (one session per step) | |
| 4b | Store pages and public site | not started | privacy, support and account deletion pages; app links working; indexing choice; landing page only if wanted | `claude --model sonnet --effort medium`, then `/phase 4b` (one session per step) | |
| 5 | Features | not started | one PR per Issue | `claude --model sonnet --effort medium`, then `/new-feature <n>` | |
| 6 | Security audit | not started | report + fixes, including the scan of the built app | `claude --model opus --effort high`, then `/phase 6` | |
| 7 | Legal, privacy and store policy review | not started | report + store privacy forms + drafts + lawyer | `claude --model opus --effort high`, then `/phase 7` | |
| 8 | Launch | not started | readiness check + docs/launch-checklist.md + store listing + runbook | `claude --model sonnet --effort high`, then `/phase 8` | |
| 9 | Cleanup review (every 5–10 features) | not started | findings + approved removal plan | `claude --model opus --effort high`, then `/phase 9` | |

Status values: not started · in progress · waiting for approval · approved.

## Current step
Phases 3, 4 and 4b run one session per step. At the end of each step, Claude rewrites this section in that step's pull request; after you merge it, type `/clear`, then `/phase <n>`. The handoff holds only what the next session can't find in the code, the Issues, the ADRs or the Decisions below: open problems, approaches that failed, facts learned the hard way. At most three lines.

- Next: none
- Handoff: none

## Store clocks
Things that take calendar time, started early so launch doesn't wait for them (docs/app-stores.md).
| What | Started on | Expected by | Status |
|---|---|---|---|
| Apple Developer Program enrollment (D-U-N-S first for a company) | | | |
| Google Play Console account and identity check | | | |
| Google Play closed test: 12 testers, 14 days in a row (new personal accounts only) | | | |
| Production access request on Google Play (after the closed test) | | | |

## Releases
<!-- One line each, newest first: version · build numbers · store build or over-the-air · date submitted · date live on each store · notes -->

## Decisions
<!-- One line each, newest first: date · decision · ADR link -->

## Open questions
<!-- Who needs to answer, and what is blocked until then -->

## Later
Things we chose not to do yet, and the event that brings each one in.
- Separate staging environment: when the first real users arrive
- Distributed tracing: when there is more than one service, or unexplained slowness
- A device farm for automated tests on many real phones: when crash reports show problems on phones we don't own
- iOS end-to-end flows on every PR: when the weekly run misses a bug that reached a release
- Tablet layouts: when SPEC.md or usage data asks for them
- Mutation testing: when the core business rules are stable
- Load tests: before a public launch or a marketing campaign
- CODEOWNERS: when a second contributor joins
- Branch protection on main: when the account moves to GitHub Pro, before launch at the latest (docs/launch-checklist.md)
