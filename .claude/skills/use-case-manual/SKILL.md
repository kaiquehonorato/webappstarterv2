---
name: use-case-manual
description: Regenerate the illustrated use-case manual in docs/manual/ with screenshots of the prototype or the running app on iOS and Android. Use after UI changes and before a release.
disable-model-invocation: true
argument-hint: "[use-case id, or empty for all]"
---
Refresh docs/manual/ for: $ARGUMENTS (empty means every use case in SPEC.md).

1. **Target**: the real app in a development or preview build, on an iOS simulator and an Android emulator, started the way the `/run` recipe or the README describes. If the real app doesn't exist yet, use prototype/ in a browser at phone size. These need a computer with the simulators (or CI); in a browser session, say so and stop after step 3 for the prototype.
2. **Walkthrough**: use the device flows Phase 3 set up (e.g. Maestro in e2e/manual/) for the app, or the Playwright walkthrough in prototype/manual/ for the prototype, or create them: one flow per use case, following the steps in SPEC.md, with seeded fake data only. Save screenshots to `docs/manual/img/<use-case>/<step>-<platform>.png` (`ios` and `android` for the app; `390` for the prototype, plus `dark` where the app has dark mode). Resize them to 390 px wide before saving. Mask or freeze dates, IDs, the status bar clock and other values that change on every run.
3. **Manual**: update docs/manual/USE-CASES.md. For each use case: the goal, who does it, numbered steps each with its screenshot and a one-line caption in the users' language (show one platform per step, and the other only where the screens differ), what the person sees when something goes wrong and how they recover, and which permissions the flow asks for. Follow docs/writing-style.md and docs/GLOSSARY.md. Write captions from the walkthrough's steps; open a screenshot only when its step changed or failed, because each one costs tokens.
4. **Truth over tidiness**: if a step fails or the screen no longer matches SPEC.md, report it as a bug. Never change the manual to hide a broken flow.
5. **Report**: the screenshots that changed, the bugs found, and the command to re-run the walkthrough.
