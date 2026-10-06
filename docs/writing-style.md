# Writing style

Every word in the product, the docs and the repo should read like a careful person wrote it for the reader in front of them. People stop trusting a product when its text sounds generated, and vague text makes them stop understanding it.

## The basics
- Write for the person's task, not about the system. Users manage "notifications", not "webhook configs".
- Be specific. Replace adjectives with facts: "Saves in under a second" beats "blazing fast".
- Plain words, short sentences, active voice. One idea per sentence.
- Sentence case for headings, buttons and labels ("Create project", not "Create Project").
- The same thing always has the same name: use docs/GLOSSARY.md everywhere, in code too.
- Numbers as digits. Dates, money and units in the user's locale.
- Every piece of text has one job. If removing it loses nothing, remove it.

## UI text patterns

| Element | Rule | Example |
|---|---|---|
| Button | Verb + object; says exactly what happens | "Save changes", "Send invite", not "Submit" or "OK" |
| Success message | Echoes the button, past tense | "Invite sent" |
| Empty state | What this place is for, plus the next action | "No projects yet. Create one to start tracking hours." + [Create project] |
| Error | What happened and how to fix it, without blame or jokes | "That e-mail is already in use. Sign in instead, or use another e-mail." |
| Destructive confirmation | Name the thing and the consequence; the button repeats the action | "Delete 'Q3 report'? Its 12 files will be removed for everyone. This can't be undone." [Delete report] [Cancel] |
| Loading | Say what is happening when it takes more than a moment | "Importing 240 contacts…" |
| Form hint | Shown before the error, about the format | "At least 12 characters" |
| Placeholder | An example, never the label | "e.g. Maria Silva" |
| Tooltip | Adds what the label can't say; never the only way to learn something essential | |

## Text that only phone apps have

| Element | Rule | Example |
|---|---|---|
| Permission explainer (the app's own screen, before the system prompt) | What the person gets, in one sentence, then the action. Shown at the moment the feature needs it | "Take photos of the unit so the client sees what was fixed." [Allow camera] [Not now] |
| iOS purpose string (`NS…UsageDescription`) | What the app does with the access, specific to this app. Vague ones get the app rejected | "The camera is used to photograph equipment during a visit." Not: "This app needs camera access." |
| Permission refused | What still works, and how to turn it on if they change their mind | "Photos are off. You can finish the visit without them, or turn on the camera in Settings." [Open Settings] |
| Push notification | Worth an interruption, specific, and private on the lock screen. Never a teaser, never personal data | "New visit added for tomorrow at 9:00." Not: "You won't believe what's new!" or the client's name and address |
| Update required screen | Why, and one action | "This version no longer works with our servers. Update to keep using the app." [Update] |
| Offline | What happened and what still works | "No connection. Your checklist is saved on this phone and will be sent when you're back online." |
| Store listing | What the app does, for whom, in the users' words. No "best", "#1" or other unprovable claims, no keyword lists, no other platforms' names (Apple's Guideline 2.3.10) | Subtitle: "Visits, checklists and photos for AC technicians" |
| What's new | What changed for the person, not for the code | "You can now attach up to 10 photos per unit." Not: "Bug fixes and performance improvements" when there is something to say |
| Review request | Only through the system prompt (StoreKit, Google Play In-App Review), after a success moment, never in exchange for anything and never filtered to happy users | |

## What makes text sound machine-written
Not banned in every context, but each one needs a reason to be there.

**Hype and filler words:** seamless, effortless, elevate, unlock, unleash, empower, supercharge, revolutionize, game-changer, cutting-edge, next-level, world-class, robust, powerful, leverage, harness, delve, embark, journey, landscape, realm, tapestry, testament, vibrant, pivotal, crucial, intricate, meticulous.

**Stock phrases:** "In today's fast-paced world", "Look no further", "Say goodbye to…", "Whether you're a … or a …", "It's not just X, it's Y", "The power of…", "Here's the thing", "Let's dive in", "In conclusion".

**Structure habits:** three adjectives or three short phrases in a row by reflex; "not only… but also"; questions as headings ("Why does it matter?"); long dashes chaining clauses; ending each section with a recap of what was just said; opening by praising the question or restating it.

**Formatting habits:** Title Case On Every Heading; bold words scattered through sentences; emoji used as bullets; exclamation marks; all-caps labels.

**Tone:** "Oops!", over-apologizing, "Please note that", "Kindly", robotic messages such as "An error has occurred", enthusiasm the situation doesn't call for.

## English (Ireland)
The default for this kit's apps. Irish English follows British spelling and day-first dates.
- Spelling: colour, centre, organise, cancelled, licence (the noun) and license (the verb), programme (but a computer program). Pick one spelling per word and keep it everywhere, the store listing included.
- Formats: €1,234.56 (euro sign first, comma for thousands, point for decimals) · 5 October 2026, or 05/10/2026 where space is short · 14:30 in the app.
- Addresses: Eircode (for example A65 F4E2) and county, not ZIP code and state. Phone numbers with +353 for people abroad. Let people type an Eircode first and fill in the rest, and don't force a county or postcode format on visitors from abroad.
- Words people use here: mobile or phone (not cell), surname, postcode or Eircode, holiday (not vacation).
- Irish (Gaeilge): only when SPEC.md asks for it, written or reviewed by a fluent speaker, never machine-translated and shipped unread.

## Portuguese (pt-BR), when the UI has it
- Natural Brazilian Portuguese with "você". Read it aloud: if it sounds translated, rewrite it.
- No gerundismo ("vamos estar enviando" → "vamos enviar").
- Prefer the Portuguese word people use: "Salvar", "Entrar", "Configurações". Keep English terms only where Brazilians really use them ("login", "e-mail").
- Sentence case: "Criar projeto", not "Criar Projeto".
- Formats: R$ 1.234,56 · 05/10/2026 · 14h30.
- The same rules for errors, permissions and notifications: "Não foi possível salvar. Verifique sua conexão e tente de novo."

## Code, commits, PRs and docs
- Comments explain why the code is the way it is, not what each line does. No comments that narrate ("This function gets the user").
- Commits: Conventional Commits with a concrete summary: `fix(auth): expire reset links after 30 minutes`, not `fix: improve authentication flow`.
- PRs: what changed, why, how it was verified (commands, output, screenshots), what's left. No "This PR enhances…".
- Docs: the shortest path to doing the thing. Commands people can copy, then the explanation.

## Before and after

| Before | After |
|---|---|
| "Unlock the full potential of your workflow with our seamless integration!" | "Connect your Google Calendar so meetings appear here automatically." |
| "Oops! Something went wrong. Please try again later." | "We couldn't load your invoices. Check your connection and select Retry." |
| "Submit" | "Create account" |
| "Are you sure?" | "Remove Ana from the team? She'll lose access to all projects right away." |
| "Welcome to your dashboard! Here you can manage everything." | "3 invoices are due this week." |
| "Delve into powerful analytics" | "See which pages people leave from" |

## Last check before shipping text
1. Would a careful person say this out loud to a customer?
2. Does each sentence tell the reader something they need?
3. Is every action named the same way on the button, in the message and in the docs?
4. Does any word on the lists above appear without a good reason?
