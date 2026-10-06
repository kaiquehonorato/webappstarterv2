# Plugin / MCP server / skill safety checklist

Plugins, MCP servers and skills run **with your permissions**. They can read your files, your code and your
credentials, and can run commands. Treat installing one like giving someone your laptop.

Real example: **Plugin4Shell** (disclosed Sept 2026) let a plugin repo owner swap the pinned, reviewed plugin code for
malicious code in Claude Code, Codex, Copilot and Gemini CLI. Anthropic fixed it in Claude Code 2.1.179.
Sources: The Hacker News, Help Net Security (see REFERENCES.md).

## Read two fields first: reach and publisher tier
Every entry in the Anthropic Directory carries these. They take five seconds to read and they decide most of the question, so read them before the checklist below.

**Reach is the blast radius.**

| Reach | What it can do | Rule |
|---|---|---|
| `contained` | Instruction files and commands only. No hooks, no network, no MCP server. It can't act until you ask it to | Fine from any tier, once you've read it |
| `remote` | Talks to a service. Your code, prompts or data leave the machine | Only for a service you already use and pay for, and only after Phase 2 chose it. Check what it sends (GDPR) |
| `privileged` | Registers hooks that run on your turns, before or after your tools, without you asking | Only at `anthropic` tier, or after reading every hook it registers, line by line |

**Publisher tier is who wrote it.** `anthropic` is Anthropic's own work. `partner` is the vendor itself, with its repo listed (e.g. `stripe`, `sentry`, `neon`). `community` is anyone else, and the author's name in the listing is a claim, not a verified identity: a plugin named after a company can sit at `community` tier. Every tier shares the same `basis: directory_review`, so **no tier is a promise that anyone audited the code**.

The dangerous combination is `community` plus `privileged`: code by an unverified author that runs on every turn. Treat that as "don't install" unless you have read all of it.

## Before installing: all must be ✅
- [ ] **Reach and tier** read, and the rule above allows it.
- [ ] **Official source first**: plugins maintained by Anthropic (`publisher.tier: anthropic`, list in docs/skills-and-plugins.md), then the vendor's own repo at `partner` tier. Being listed in the directory is not an audit.
- [ ] **Needed now**: the current phase uses it. Install later plugins later.
- [ ] **Exact name matches** the official repo/org (watch for look-alikes: `hyper-link`, `hyperlinkk`, forks with a different owner). The listing's `upstream.repo_url` is the name that counts; an entry with no upstream repo is code you cannot read before installing, which fails the next check.
- [ ] **Reputation**: known author/company, real stars + contributors, recent commits, open issues answered, not created last week.
- [ ] **Read the code / manifest**: what hooks does it register? What commands does it run? Does it send data to external URLs? Any obfuscated or minified code, `curl | sh`, postinstall scripts?
- [ ] **Permissions**: does it need them? A formatter doesn't need network access; a docs helper doesn't need to run shell commands.
- [ ] **Data**: where does your code/prompt go? Read the privacy policy if it calls a hosted service. Never send client or personal data to an unknown service (GDPR).
- [ ] **License** allows your use (commercial?).
- [ ] **Search for news**: "<name> vulnerability", "<name> malware", "<name> security".

## After installing
- [ ] Keep Claude Code updated. This kit needs 2.1.284 or later, which includes the Plugin4Shell fix.
- [ ] Prefer GitHub-hosted plugins; disable auto-update for third-party marketplaces; review changes on update.
- [ ] Try it first in a throwaway project, not your main repo with real `.env` files.
- [ ] Keep secrets out of reach (`.claude/settings.json` deny rules in this kit).
- [ ] Remove plugins you no longer use (fewer tools = less risk and fewer tokens). `/skill-doctor` and `/doctor` show what each one costs in context and which ones you never use.

## Copying a skill instead of installing it
A skill that is only instruction files can be copied into `.claude/skills/<name>/`, the way this kit carries `frontend-design`, two Superpowers skills and Cloudflare's `security-audit`. The copy works in browser sessions, where plugins don't load, and never changes unless you change it.
- [ ] Read every file you copy. Leave out anything you don't need, especially scripts.
- [ ] Prefer the copy even when the skill ships an installer. `npx skills add` and `curl | sh` fetch and run their newest version every time, so what you reviewed is not what runs next month.
- [ ] The license allows copying (MIT, Apache-2.0); keep the license file in the skill's folder.
- [ ] Record the source, version or commit, and anything you changed, in docs/skills-and-plugins.md.
- [ ] Turn off the plugin version, if you had it installed, so the skill doesn't load twice.

## Red flags: don't install
- `community` tier with `privileged` reach, and you have not read every hook.
- No `upstream.repo_url` in the listing, so you cannot read the code before it runs.
- Installs packages at session start, or runs an unpinned `npm install`, `pip install` or `curl | sh`.
- Asks you to paste API keys/passwords into a prompt or config it controls.
- Promoted only by viral posts/ads, with no clear author.
- Closed source but asks for shell or file access.
- Popularity or "it's working for everyone" as the only argument. Success ≠ safety.
