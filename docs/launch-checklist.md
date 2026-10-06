# Launch checklist: what only you can do

Account settings, store forms, signing keys, payments and outside reviews. They live outside the code, so Claude can't change or check them for you. Claude adds items here as the project goes (Phase 3 adds the branch rules and the signing setup, for example), and in Phase 8 it goes through the list with you and records your answers. Tick each box in the pull request that finishes Phase 8.

## Store accounts
- [ ] The Apple Developer Program and Google Play Console accounts belong to the product's owner (the company's organization accounts for a company app), not to a freelancer or a personal account that might leave.
- [ ] Two-step sign-in on both, with recovery options that more than one trusted person can reach.
- [ ] Everyone else works as an invited user with the smallest role that does the job (in App Store Connect, Developer or App Manager rather than Admin; in Play Console, permissions per app). Nobody shares the owner's password.
- [ ] API keys for uploads from CI (App Store Connect API key, Google Play service account) are scoped to what CI needs, stored only in the build service or CI secrets, and listed somewhere you can find them to rotate.
- [ ] Banking, tax and agreements are complete in both stores if the app sells anything; the paid apps agreement is accepted in App Store Connect.
- [ ] Digital Services Act trader status declared in both stores with the company's details. As an EU business you are a trader, and the stores show the address, phone number and e-mail on the listing in the EU, so use the business contacts, not personal ones.
- [ ] If your Google Play account is a new personal one: the 14-day closed test with 12 opted-in testers is done and production access is granted.

## App signing
- [ ] Play App Signing is on, so Google keeps the app signing key and a lost upload key can be reset.
- [ ] The Android upload key and its password are backed up outside the build service, in a password manager or a safe place only you control.
- [ ] Apple certificates, provisioning profiles and the push notification key live in the build service or your Apple account, never in the repository.
- [ ] Nobody, including Claude, has a copy of a signing key on a laptop that isn't needed there.

## GitHub
- [ ] Your account is on GitHub Pro, about US$ 4 a month (GitHub Team for an organization). On the free plan, a private repository can't protect its main branch.
- [ ] A ruleset protects main: in the repository, **Settings → Rules → Rulesets → New branch ruleset**. Set "Enforcement status" to Active, target the default branch, then turn on "Require a pull request before merging", "Require status checks to pass" (choose the CI checks), "Block force pushes" and "Restrict deletions".
- [ ] If production deploys or store uploads run from GitHub Actions, their secrets live in a GitHub environment named `production` that only main can deploy to (**Settings → Environments**). Pro enables this for private repositories.
- [ ] gitleaks and Semgrep still run in CI. GitHub's secret scanning and CodeQL don't run on a private repository owned by a personal account, even on Pro.

## Two-step sign-in on every account
Use an authenticator app or a passkey rather than SMS, and keep the recovery codes offline.
- [ ] GitHub
- [ ] Apple Developer and App Store Connect
- [ ] Google Play Console and the Google account behind it
- [ ] The build service (e.g. Expo, Codemagic), if you use one
- [ ] Hosting (e.g. Netlify, Vercel, Fly)
- [ ] Database (e.g. Supabase)
- [ ] Domain registrar and DNS
- [ ] Payments (e.g. Stripe), if the product charges
- [ ] Push notifications, error monitoring, e-mail sending and any other service that holds user data
- [ ] The e-mail account behind all of the above, since password resets go through it

## Store forms
- [ ] App Privacy (Apple) and Data safety (Google) entered from the Phase 7 drafts, after review, and matching what the release build and its SDKs really collect.
- [ ] Age rating questionnaires answered on both stores; Google Play's target audience and content declarations filled in.
- [ ] Privacy policy, support and account deletion URLs entered, and each one loads.
- [ ] Review notes and the demo account (fake data only) entered in App Store Connect and in Play Console's "App access".
- [ ] Export compliance (encryption) answered in App Store Connect.

## Backups
- [ ] The backup plan from Phase 2 is switched on. On Supabase, for example, Free has no automatic backups, Pro keeps 7 days of daily backups, and point-in-time recovery is a paid add-on.
- [ ] Files have their own backup. Supabase's database backups don't include files kept in Storage.
- [ ] An off-site copy runs on a schedule if the plan calls for one (e.g. `supabase db dump` to storage you control).
- [ ] A restore was tested into a scratch database and timed. Phase 8 does this with you.

## Money and domain
- [ ] Budget alerts on every paid service, including the build service and CI minutes.
- [ ] The domain renews automatically and the registrar lock is on. The app links and the store pages depend on it.

## Store pages and public site (Phase 4b)
- [ ] The privacy policy, support and account deletion pages load on a phone, without signing in, and name the app as the store listing does.
- [ ] A test account deleted through the web page is really gone, and the same through the app.
- [ ] Links open the app on an iPhone and an Android phone, and open the site when the app isn't installed.
- [ ] The link preview is right: paste the URL into WhatsApp and look at the image, title and description.
- [ ] Nothing on the pages or in the store listing claims a number, a download count, a rating, a review, a customer, an award or a certification we can't prove, and no logo is used without written permission.
- [ ] If there is a consent banner, rejecting it really stops the third parties loading, checked in the browser's network tab, and the choice can be changed from the footer.
- [ ] The prices shown are the prices charged, on the site and in the app.
- [ ] Indexing matches what you chose in Phase 4b: for a public app, `robots.txt` and `sitemap.xml` are the production ones; for an internal app, every page carries `noindex` and `robots.txt` disallows everything but the `.well-known` files. No preview or staging URL is indexable either way.
- [ ] The only analytics or tracking is the one you asked for, and nothing was added to "measure the launch".

## Data protection and product rules (Ireland and the EU)
- [ ] The records of processing exist, and a data protection impact assessment is done if Phase 7 said one is needed.
- [ ] The data protection contact is published; if a Data Protection Officer is required (GDPR Art. 37), their contact is published and notified to the DPC.
- [ ] Someone knows where the DPC's breach notification form is and who files it within 72 hours, and where ENISA's Single Reporting Platform is for the Cyber Resilience Act's 24-hour warning (SECURITY.md).
- [ ] Each processor that holds personal data has a signed data processing agreement, with a transfer mechanism if data leaves the EEA.
- [ ] If the app sells to consumers: the withdrawal function works, and the business details (name, address, e-mail, company and VAT numbers) are on the site. If the European Accessibility Act applies, the accessibility statement is published.

## Outside review
- [ ] A lawyer (a solicitor, in Ireland) reviewed the privacy notice, the terms and the account deletion text (Phase 7).
- [ ] With payments, health or ID data, or users under 18: a professional security review or penetration test of the app and the API is done and its findings are fixed.

## Added during the project
<!-- Claude adds items here: phase · what to do · why. Example: "Phase 3 · invite 12 testers to the closed test with the first preview build · a new personal Google Play account can't publish without it" -->
