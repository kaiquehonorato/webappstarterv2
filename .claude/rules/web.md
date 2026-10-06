---
paths:
  - "apps/site/**"
  - "apps/web/**"
  - "**/public/.well-known/**"
  - "**/*.{vue,svelte,css,html}"
---

# Web rules: store pages, public site and any web version
Loaded only when Claude reads matching files. Update the `paths` above in Phase 2 to match the real folders.

- The pages the stores require (privacy policy, support, account deletion request) are plain, fast and always reachable: server-rendered HTML that works without JavaScript, no sign-in wall in front of them, and the app's name exactly as the store listing shows it.
- `/.well-known/apple-app-site-association` and `/.well-known/assetlinks.json` are served over HTTPS with `Content-Type: application/json`, without redirects, and name only our app IDs and signing certificate fingerprints. A test fetches both from the built site.
- Render on the server by default; ship client-side JavaScript only where interaction needs it. Never call the database or use secrets from the browser; `NEXT_PUBLIC_*` and `VITE_*` values are public.
- Every async view has loading, empty, error and success states; forms validate with the shared zod schema on the client for feedback and again on the server for security.
- Use `packages/ui` tokens: the site and the app are one product, with no second palette or font family.
- Motion: 150–300 ms, transform and opacity only, no layout shift, and a reduced-motion version (`prefers-reduced-motion`). Scroll effects in CSS (`animation-timeline: view()` or `scroll()`), one reveal pattern, never on the LCP element, content readable at the animation's start value, no scroll hijacking.
- Never render untrusted HTML without the project's sanitizer.
- Accessibility, WCAG 2.2 AA: semantic HTML, keyboard access with a visible focus ring, labels on inputs, 4.5:1 text contrast, targets of at least 24×24 px (44×44 on touch screens), no information conveyed by color alone. End-to-end tests include an axe check.
- Responsive from 360 px up; check 390 and 1280 px with screenshots before calling web work done.
- Performance budget on mobile (75th percentile): LCP < 2.5 s, INP < 200 ms, CLS < 0.1, plus the JavaScript size limit CI enforces per page.
- Nothing third-party loads before consent, and analytics is a question for the owner, not a default (prompts/04b-public-site.md).
- Store badges follow Apple's and Google's badge guidelines: their artwork, unmodified, linking to the real listing.
- Text follows docs/writing-style.md and uses the words in docs/GLOSSARY.md. With more than one language, all text lives in the i18n files.
