---
paths:
  - "apps/mobile/**"
  - "packages/ui/**"
  - "**/screens/**"
  - "**/*.{dart,swift,kt}"
  - "**/app.json"
  - "**/app.config.*"
  - "**/Info.plist"
  - "**/AndroidManifest.xml"
  - "**/*.entitlements"
  - "**/eas.json"
---

# Mobile app rules
Loaded only when Claude reads matching files. Update the `paths` above in Phase 2 to match the real folders and stack.

**Security on the device**
- The app is public: no secret keys, admin endpoints or business rules that matter for money or permissions. Anything in the bundle, `Info.plist`, `strings.xml` or a public env var (`EXPO_PUBLIC_*`, `--dart-define`) can be read by anyone.
- Tokens, keys and sensitive data only in secure storage (Keychain, Android Keystore, e.g. `expo-secure-store` or `flutter_secure_storage`). Never in AsyncStorage, SharedPreferences, UserDefaults, plain files, logs, crash reports, analytics events or the clipboard. Exclude sensitive files from device backups.
- Sign-in through the provider's SDK or OAuth in the system browser (ASWebAuthenticationSession, Custom Tabs) with PKCE; never a login form inside a WebView. Biometrics unlock a token already stored; the server still authenticates.
- Deep links, universal links, app links, push payloads and Android intents are untrusted input: validate every parameter with the shared schema, never let a link perform an action without the user confirming it, and prefer verified links (universal links, app links) over custom URL schemes for anything sensitive, such as sign-in callbacks.
- Android: export only the components that must be reachable from other apps, with permissions where needed; `debuggable` off and backups limited in release builds. iOS: no App Transport Security exceptions without an ADR.
- WebViews only for content you control: JavaScript off unless needed, no file access, no native bridge exposed to pages from other origins, and links to other sites open in the system browser.
- Release builds compile out debug menus, verbose logging, development endpoints and test accounts. `console.log` and print statements carry no personal data in any build.
- Hide sensitive screens from the app switcher and screenshots where the threat model says so (`FLAG_SECURE` on Android, a cover view on iOS).
- Push notifications show nothing private on the lock screen: "You have a new message", not the message.

**Permissions and privacy**
- Ask for a permission at the moment the feature needs it, after a short screen that says why, never at first launch. Handle "denied" and "denied forever" with a way forward (the feature without it, or a link to Settings).
- Every iOS purpose string (`NS…UsageDescription`) says what the app does with the access, in the users' language. Vague ones get the app rejected.
- No new permission, entitlement or SDK without approval; each one changes the store privacy forms (docs/app-stores.md).

**Screens and states**
- Every async screen has four states: loading (skeleton after 300 ms), empty (what this place is for and the next action), error (what happened, how to recover, retry) and success, plus offline: what still works without a connection and what the person sees when it returns. Quick actions update optimistically and roll back on failure.
- Use `packages/ui` components and tokens only: no hard-coded colors, spacing, fonts, shadows, or one-off buttons and animations.
- One primary action per screen. Button labels say what happens ("Save changes", not "Submit"); the toast or snackbar echoes it ("Changes saved").
- Reversible actions get undo; irreversible ones get a confirmation that names what will be lost.
- Forms validate with the shared zod schema on the device for feedback and again on the server for security. Errors say how to fix the problem. Inputs use the right keyboard and autofill type (e-mail, phone, one-time code, password), stay visible above the keyboard, and "Next" moves to the next field.
- Follow each platform's conventions where people expect them: Android's back button and gesture always work and never trap the person; iOS swipe-back works on pushed screens; native pickers, share sheets and alerts over home-made ones.
- Long lists are virtualized and paginated; scrolling never drops frames on the cheapest phone in the device list.

**Accessibility (WCAG 2.2 AA, applied to native screens)**
- Every control has an accessible name and role for VoiceOver and TalkBack; images that carry meaning have a description; decorative ones are hidden from screen readers.
- Text follows the phone's text size setting up to 200% without clipping or overlapping; layouts reflow instead of truncating.
- Touch targets at least 44×44 pt on iOS and 48×48 dp on Android; text contrast 4.5:1 (3:1 for large text and UI parts); no information conveyed by color alone.
- Every gesture (swipe to delete, long press, drag) has a visible alternative.
- Motion: 150–300 ms for feedback, transform and opacity only, and the OS "reduce motion" setting turns it off. Haptics confirm, they never carry information alone.

**Performance and size**
- Budgets from docs/architecture.md, checked on a low-end Android phone: cold start, time to the first useful screen, scrolling without dropped frames, memory, app download size.
- Images sized for the screen and cached; nothing heavy on the startup path; work that can wait runs after the first screen.

**Text**
- Follows docs/writing-style.md and uses the words in docs/GLOSSARY.md. With more than one language, all UI text, purpose strings and notification text live in the i18n files.
