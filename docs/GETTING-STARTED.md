# Getting started

This guide takes you from an empty repository to an approved spec, then shows how every later phase works, up to the first store release. You can run Claude Code in a terminal on your computer or in the browser at [claude.ai/code](https://claude.ai/code). Planning phases work the same in both; anything that needs an iOS simulator, an Android emulator or a phone runs on your computer.

## What you need

- A Claude plan that includes Claude Code: Pro, Max, Team, Enterprise, or a Console (API) account. The free plan doesn't include it.
- A GitHub account with two-step sign-in on (GitHub → Settings → Password and authentication). Your code, Issues and pull requests live there. The free plan is enough to start. Before launch you'll move to GitHub Pro, about US$ 4 a month, because a private repository on the free plan can't protect its main branch; `docs/launch-checklist.md` keeps that step and the other account settings for the end.
- The store accounts, started early because verification takes time (details and prices in `docs/app-stores.md`):
  - Apple Developer Program, US$ 99 a year. Needed from the first test build on an iPhone (Phase 3).
  - Google Play Console, US$ 25 once. A new personal account must run a 14-day closed test with 12 testers before it can publish, so open it before Phase 3 and start the test with the first preview build.
  - For a company app, enroll as the company (both stores need a D-U-N-S number), not with a personal account.
- One iPhone and one Android phone for testing; an older, cheaper Android phone shows performance problems a new one hides.

### For the terminal (needed from Phase 3)

1. Git, from [git-scm.com](https://git-scm.com). On Windows, install Git for Windows so Claude can run Bash commands.
2. Claude Code:
   ```bash
   # macOS, Linux, WSL
   curl -fsSL https://claude.ai/install.sh | bash
   ```
   ```powershell
   # Windows PowerShell
   irm https://claude.ai/install.ps1 | iex
   ```
   Open a new terminal and run `claude --version`. Then run `claude` once and follow the browser prompts to log in. This install updates itself. The kit needs version 2.1.284 or later; `claude update` updates right away.
3. The GitHub CLI, from [cli.github.com](https://cli.github.com). Then run `gh auth login`. Claude uses it to open Issues and pull requests.
4. Node.js, the LTS version from [nodejs.org](https://nodejs.org). The Phase 1 prototype, the backend and most app frameworks need it.
5. Android Studio, from [developer.android.com/studio](https://developer.android.com/studio), for the Android emulator and SDK.
6. On a Mac: Xcode from the Mac App Store, for the iOS simulator and iOS builds. Without a Mac, Phase 2 picks a cloud build service, and you test iOS builds on your iPhone through TestFlight instead of a simulator.

Phase 3 pins the exact versions the chosen framework needs (Xcode, JDK, Android SDK) and writes them into CONTRIBUTING.md.

### For the browser

Nothing to install. Open [claude.ai/code](https://claude.ai/code), connect GitHub when asked, and install the Claude GitHub App on your GitHub account. A private repository shows up in Claude Code only when the app has access to it. Each browser session runs on a cloud machine that clones your repository from GitHub, so Claude sees only what you have pushed. There Claude opens Issues and pull requests through that GitHub connection instead of the GitHub CLI.

That cloud machine runs Linux with no simulator or emulator. It is good for Phases 0, 2, 6, 7 and 9, for backend work and for unit tests; for screens and device checks, use the terminal or a **Local** session in the Claude desktop app, which behaves like the terminal. Claude says which checks it could not run.

## Make the kit a template (once)

1. On GitHub, open the kit's repository and go to **Settings**.
2. Under the repository name, check **Template repository**.

From then on, each project starts as a copy of the kit in one click. Changing this setting needs admin rights on the kit's repository.

## Step 1. Create the project's repository

1. On the kit's GitHub page, click **Use this template** → **Create a new repository**.
2. Give it a name (for example `field-visits-app`) and choose **Private**, so your spec and code aren't public.
3. Then:
   - Terminal: clone it and open the folder.
     ```bash
     git clone https://github.com/<your-user>/field-visits-app.git
     cd field-visits-app
     ```
   - Browser: if the Claude GitHub App only has access to selected repositories, add the new one. On GitHub, go to **Settings** → **Applications** → **Installed GitHub Apps** → Claude → **Configure** → **Repository access**.

The new repository comes with every rule, agent, skill, prompt and guide in the kit. From the first session, `.claude/settings.json` blocks Claude from reading `.env` files, signing keys and store credentials, force-pushing, pushing to main and running `rm -rf`. It also makes Claude ask before `git reset --hard`, database resets, production deploys, store submissions and over-the-air updates. The kit's skills are files in `.claude/skills/`, so they work the same in the terminal and in the browser, with nothing to install: `frontend-design` for the look of every screen, two skills from Superpowers for finding a bug's root cause and for checking before "done", Cloudflare's `security-audit`, and the kit's own `/new-feature`, `/store-release` and `/use-case-manual` (docs/skills-and-plugins.md).

## Step 2. Describe your idea

1. Open `prompts/00-kickoff.md` and copy everything inside the `text` block into a text editor.
2. Replace the 11 lines under "The idea". Short answers are fine, and "not sure" counts as an answer: Claude will ask about it.

A filled-in example:

```text
## The idea
- What it is: An app for the technicians of a small heating and air-conditioning service company in Cork. They see the day's visits, follow a checklist for each unit, take photos of the equipment, and the client signs on the phone at the end. Today it is paper forms and WhatsApp photos, and reports take two days to reach the client.
- Users: About 15 technicians and 2 office staff. Adults only.
- Where: The company is registered in Ireland; users are its staff in Munster; English only.
- Platforms: Android first (company phones), iPhone later. Not sure.
- Who installs it: Only the company's staff.
- Phone features it needs: Camera, works offline in basements, notifications for new visits.
- Store accounts: None yet. The company is registered with the CRO.
- Already have: GitHub, a Supabase account, a Mac, an old Android phone.
- Limits: Up to €300 per month; about 10 hours a week; a pilot with 5 technicians in 3 months.
- My experience: Some coding.
- Talk to me in: Português
```

Four of these answers affect the plan the most:
- Who installs it. An app only for staff can skip the public store listing, the landing page and the search engines, and use one of the private routes in `docs/app-stores.md`. A public app needs all of them.
- Platforms. Each platform adds testing, review and store work. Starting with one is a real option.
- Users and where they are. Claude asks first which country the business is established in, with Ireland as the default: the GDPR and Irish law then apply, and users in other countries add their own rules. Name another country and Claude tells you which of the kit's legal rules change. If children could use the product, even by accident, extra protections apply (in Ireland, under-16s can't consent to online services themselves), and Claude asks you before building anything for them.
- Limits. Your budget and hours decide the framework, the build service, the hosting and the v1 scope Claude recommends.

In the terminal you can skip the copying. Fill in the lines in the file itself, save it, and start the session with `Follow the prompt in @prompts/00-kickoff.md`.

## Step 3. Start Phase 0

Terminal, from the project folder:

```bash
claude --model opus --effort high
```

Then paste the filled-in prompt as your first message.

Browser: at claude.ai/code, choose the repository in the selector below the message box. Send `/model opus`, then `/effort high`, then paste the prompt.

Phase 0 runs on Opus at high effort because the scope and risk decisions made here shape every later phase. In the terminal, the flags apply only to this session; your default model and effort stay as they were.

## Step 4. The discovery conversation

Expect 1 to 2 hours of conversation. Claude writes no code in this phase.

1. Reality check. Claude restates your idea in three lines and first asks whether it needs to be a store app at all, comparing it with a web app that works well on phones. For the example above, the camera, offline use in basements and notifications argue for an installed app. Then it names the riskiest assumption (that technicians will fill a checklist on a phone at the end of a hot day), the smallest version that would test it, and what usually goes wrong with this kind of product.
2. Interview. Questions come a few at a time, each with a recommended answer. They cover roles and permissions, platforms and how the app is distributed, store accounts, the main use cases, permissions and offline use, personal data, money, look and feel, abuse and risk, and who handles problems once the app is live.
3. Choosing v1. Claude offers three scopes (minimal, balanced, ambitious) with rough effort, monthly cost, one-time costs and main risk for each, and recommends one. You choose.
4. Documents. Claude writes `SPEC.md`, `docs/GLOSSARY.md` and `docs/ROADMAP.md`.
5. Gate. Claude stops and gives you a 10-line summary, the open questions, what it would cut first if time runs short, and which store accounts to start enrolling in now. Read `SPEC.md` before you approve, because every later phase builds on it. Ask for changes until it's right, then reply "approved".

After you approve, Claude opens a pull request with the documents. Open it on GitHub, read the **Files changed** tab, then click **Merge pull request** and **Confirm merge**. Claude finishes by telling you which prompt, model and effort to use for Phase 1.

While you answer:
- "Use your recommendation" is a valid answer when you have no opinion.
- Paste screenshots of apps you like into the chat. They help more than adjectives.
- If the interview drifts, say "Summarize what we have and ask only what blocks the spec."

## Step 5. Every phase after that

Each phase runs the same loop:

1. Start a fresh session with that phase's model and effort (table below).
2. Type `/phase <n>`, for example `/phase 1`. It reads the phase's prompt from `prompts/`, so you don't have to paste it (pasting still works).
3. Answer Claude's questions and approve its decisions.
4. Claude does the work and shows evidence: tests, screenshots from both platforms, command output.
5. At the gate, check the evidence, approve, and merge the pull request.

Phases 3, 4 and 4b run this loop once per step. After you merge a step's pull request, type `/clear` (in the browser, start a new session), then `/phase <n>` again. Claude leaves a short handoff in `docs/ROADMAP.md`, so the next step starts with a clean context. A short session per step costs less than one long session, and Claude follows the rules better with less in its context.

| Phase | Start the session with | Prompt | What you do |
|---|---|---|---|
| 1 Prototype and manual | `claude --model sonnet --effort medium` | `prompts/01-prototype-manual.md` | Share 2–3 screenshots of apps you like, pick one of three visual directions, open the prototype on your phone and use it for a day, read the illustrated manual |
| 2 Architecture | `claude --model opusplan --effort high --permission-mode plan` | `prompts/02-architecture.md` | Choose the app framework, backend, hosting, database, sign-in and build service from options with monthly costs; approve the data model; decide each item in the table of what the documents disagree on |
| 3 Foundation | `claude --model sonnet --effort medium`, on your computer | `prompts/03-foundation.md` | Approve one pull request per step; choose the app's identifiers (they can never change); enter signing credentials and secrets yourself in the build service or GitHub; install the first preview build on your phones; invite the closed-test testers if your Google account needs them |
| 4 Design system | `claude --model sonnet --effort medium`, on your computer | `prompts/04-design-system.md` | Check the component catalog in light and dark, at the largest text size, on your own phones; approve the app icon |
| 4b Store pages and public site | `claude --model sonnet --effort medium` | `prompts/04b-public-site.md` | Say whether the app is for the public or for staff, and whether you want a landing page; try the account deletion page on a test account; check a link opens the app; check the link preview in WhatsApp |
| 5 Features | `claude --model sonnet --effort medium`, then `/new-feature <issue number>` | `prompts/05-feature.md` (what to do when a feature gets hard) | One Issue per session: review the screenshots and evidence in the pull request, then merge |
| 6 Security audit | `claude --model opus --effort high` | `prompts/06-security-audit.md` | Every 3–5 features and before launch: approve the fixes, and set the agent budget for the deeper hunt |
| 7 Legal and store policy | `claude --model opus --effort high` | `prompts/07-legal-review.md` | Take the report and drafts to a lawyer; enter the privacy labels, Data safety answers and age ratings in the stores after review |
| 8 Launch | `claude --model sonnet --effort high`, on your computer | `prompts/08-launch.md` | Go through `docs/launch-checklist.md` (store accounts, signing, GitHub Pro, two-step sign-in, backups, outside reviews), check the release build on your phones, then approve each submission and rollout step |
| Every release | `claude --model sonnet --effort medium`, then `/store-release <version>` | `.claude/skills/store-release/SKILL.md` | Check the test build on your phones, approve the submission, watch the rollout numbers with Claude |
| 9 Cleanup review | `claude --model opus --effort high` | `prompts/09-cleanup-review.md` | Every 5–10 features: approve the removal plan, then merge one small PR per step |

These are starting points. If Claude skips work, raise the effort one step; if it tries hard and is still wrong, switch to Opus. `docs/models-and-tokens.md` has the full guide, from medium to max, and the level to raise each phase to.

In the browser, start each phase as a new session from the sidebar and send `/model` and `/effort` with the values from the table as your first messages. For Phase 2, also choose **Plan** in the mode menu next to the message box.

Phases 1 and 4 use the `frontend-design` skill, which ships in `.claude/skills/`, so the design guidance is the same in the terminal and in the browser. Plugins are optional extras for the terminal (`docs/skills-and-plugins.md`).

## Habits that keep quality high and cost low

1. One phase, one step (Phases 3, 4 and 4b) or one Issue per session. In the terminal, `/clear` between unrelated tasks; in the browser, start a new session. The plan lives in files (`SPEC.md`, `docs/ROADMAP.md`, the ADRs), so a fresh session loses nothing.
2. If you correct Claude twice on the same problem and it's still wrong, stop. Start a fresh session and say what went wrong and what you want instead.
3. Don't accept "done" without evidence: a test, a screenshot from each platform, or a command and its output. Ask which checks ran on a real phone.
4. Never paste passwords, API keys, signing keys or store credentials into the chat. They go in your hosting provider's settings, in the build service, in GitHub secrets, or in a local `.env` file you edit yourself.
5. `docs/ROADMAP.md` always shows where the project stands and what comes next.
6. In the terminal, press Esc to stop Claude mid-task. Press Esc twice, or type `/rewind`, to go back to an earlier point.
7. To pick up later, run `claude --continue` to reopen the last session in the folder, or `claude --resume` to choose one. In the browser, reopen the session from the sidebar. After a break of more than about an hour, a fresh session costs less and works just as well.
8. Read `docs/vibe-coding-mistakes.md` once. It lists the 52 most common ways AI-built projects and store apps fail, and the guardrail this kit uses against each.
9. Questions about the kit: `/ask <question>` answers from the docs on a small model in its own context, so only the answer enters the conversation. `/btw` answers a quick side question without adding it to the conversation.

## When something goes wrong

| What you see | What to do |
|---|---|
| `claude: command not found` right after installing | Open a new terminal window. If that doesn't help, run `claude doctor` |
| Your repository isn't listed at claude.ai/code | Give the Claude GitHub App access to it (Step 1) |
| Claude asks permission to run a command | Read the command first. Approve normal development commands. Decline anything that deletes data, touches production or submits to a store unless you asked for it |
| Claude says it can't read `.env` or a key file | That's on purpose: the kit blocks it so secrets and signing keys never reach the chat. Fill them in yourself; Claude works from `.env.example` |
| Claude says it couldn't run the simulator | You are in a browser session, or the simulator isn't installed. Run that check on your computer, or let CI run it |
| The app works on the simulator but not on your phone | Ask Claude to reproduce it on the phone first (systematic-debugging). Release builds, slow phones and real networks behave differently |
| A store rejected the submission | Paste the rejection text into a fresh session on Opus and ask for the guideline it cites, the smallest fix and the reply to the reviewer |
| The same mistake keeps coming back | Start a fresh session with a better prompt (habit 2) |
| The session gets slow or loses track | Type `/context` to see what fills it, then `/compact` and what to keep, or start a fresh session |
| A decision doesn't make sense to you | Ask "Explain this as if I'm new to it" or "Show me the ADR for this" |

## How long it takes

These are rough figures. Your scope decides the total, and Claude estimates the effort of each scope option in Phase 0.
- Phase 0: one session, 1 to 2 hours of conversation.
- Phases 1 and 2: one or two sessions each.
- Phases 3, 4 and 4b: one session per step (7, 7 and 11 steps).
- Each feature: one session, often 30 to 90 minutes.
- Store clocks that run in parallel: account verification (days to weeks), Google's 14-day closed test for new personal accounts, and review before each release (a day or two is common).

## Read next
- `README.md`: the phases at a glance.
- `docs/app-stores.md`: accounts, costs, distribution and the store rules that shape the app.
- `docs/models-and-tokens.md`: when to change model or effort, and how to spend fewer tokens.
- `docs/skills-and-plugins.md`: built-in commands and the plugins worth installing.
- `docs/security-model.md`: what "secure" means for an app on someone else's phone, and how it tests itself.
