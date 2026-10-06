# App stores: accounts, costs, distribution and the rules that shape the app

What publishing on the App Store and Google Play involves, gathered in one place so it shapes the plan from Phase 0 instead of surprising you at submission. Checked against Apple's and Google's official pages on 6 October 2026. Store rules change several times a year, so Phases 7 and 8 and `/store-release` check the official pages again instead of trusting this file. Links are in docs/REFERENCES.md.

## Accounts and what they cost

| | Apple | Google |
|---|---|---|
| Account | Apple Developer Program | Google Play Console |
| Price | US$ 99 a year | US$ 25, once |
| For a company | An organization account needs a D-U-N-S number, which is free but can take days to weeks | An organization account needs a D-U-N-S number too |
| Identity check | Yes, before the account works | Yes, developer verification |
| Before the first public release | Nothing extra | A **personal** account created after 13 November 2023 must run a closed test with at least 12 testers opted in for 14 days in a row, then apply for production access (Google says that review usually takes 7 days or less). Organization accounts are exempt |
| Commission on sales in the app | 15% under the Small Business Program (under US$ 1 million a year) and on subscriptions after their first year; 30% otherwise | 15% on the first US$ 1 million a year and on subscriptions; 30% above |

Who should own the accounts: whoever owns the product. For a company app, that is the company's organization account, with the people who work on it added as users with the smallest role that does their job. An app published from a freelancer's personal account can only move to the company through a transfer that both sides have to complete, and one that disappears with its owner can't be updated at all.

"Opted in" on Google's closed test means each tester opened the test link, accepted, and installed the app from Google Play with that same Google account; adding an e-mail to the list is not enough. Start this test with the first preview build in Phase 3, not in launch week.

## Tools and devices
- iOS apps are built with Xcode, which runs only on a Mac. Without a Mac, a cloud build service (for example EAS Build or Codemagic) or GitHub Actions' macOS runners build them for you; macOS minutes cost several times more than Linux ones. The iOS simulator also needs a Mac.
- Android apps build on any operating system; the emulator comes with Android Studio.
- Real phones for release checks: at least one iPhone and one low-end Android phone. A simulator is not a phone: performance, the camera, notifications and sign-in with the system browser behave differently.
- A browser session at claude.ai/code runs on a Linux machine in the cloud with no simulator. Device checks run on your computer or in CI.

## Ways to distribute

**Public**: the App Store and Google Play listings, found by search, after review.

**Only for one company's staff.** Testing channels are not a way to run a company: TestFlight builds expire after 90 days, and testing tracks have tester limits. The real options:

| Route | How it works | Fits when |
|---|---|---|
| Apple custom app (Apple Business Manager) | Reviewed by Apple like any app, then offered privately to the organizations you name, through their Apple Business Manager | The company uses Apple Business Manager, usually with device management |
| Apple unlisted app | Reviewed, then reachable only through a direct link, never in search | Staff use their own iPhones and the company has no device management |
| Apple Developer Enterprise Program | In-house distribution without App Review, US$ 299 a year, with strict eligibility for large organizations | Rarely the right answer; ask before considering it |
| Private app in managed Google Play | Published from Play Console to specific organizations that use Android Enterprise | The company manages its Android phones |
| An APK installed outside Google Play | Distributed by link or by device management | Only with developer verification once it reaches Ireland (see below), and with the security checks the store would otherwise do |

Permanently private apps for one organization are exempt from Google Play's target API level rule. Everything else in this kit still applies to internal apps: security, the GDPR, accessibility, account deletion if people create accounts.

## What review checks, and the rules that shape the app
The guideline numbers are Apple's App Review Guidelines; Google's rules are in its Developer Program Policies.

| Rule | Apple | Google | Where the kit handles it |
|---|---|---|---|
| Account deletion | Inside the app, if the app lets people create an account (5.1.1(v)) | Inside the app **and** from a web page that works without the app, declared in the Data safety form | Phases 0, 2 and 4b; the deletion service in the backend rules |
| Privacy policy | A URL in App Store Connect and a link inside the app (5.1.1(i)) | A URL in Play Console and inside the app | Phases 4b and 7 |
| Support | A support URL in App Store Connect | A contact e-mail | Phase 4b |
| What data the app collects | App Privacy details ("privacy labels"), plus privacy manifests declaring the reasons for certain APIs, in our code and in every SDK (required since 1 May 2024) | The Data safety form, covering every SDK | Phase 7, and `/store-release` whenever data changes |
| Tracking | App Tracking Transparency permission before tracking across other companies' apps and sites (5.1.2(i)) | Advertising ID rules and disclosure | Phases 2 and 7 |
| Social sign-in | Offering Google, Facebook or similar sign-in also requires an equivalent option that limits data to name and e-mail, lets people hide their e-mail and doesn't track them, such as Sign in with Apple (4.8) | No equivalent rule | Phase 2 |
| Payments | Digital content and features are sold through in-app purchase (3.1.1); on the US storefront apps may also link to purchases outside the app (3.1.1(a)); physical goods and services use other payment methods | Google Play's billing for digital goods, with exceptions that vary by country | Phases 0, 2 and 7 |
| Reviewer access | A demo account or a full demo mode, with the backend live during review (2.1(a)) | "App access" instructions in Play Console | Phase 8 |
| Minimum functionality | Not a repackaged website (4.2) | Spam and minimum functionality policy | Phases 0 and 2 |
| Code updates | Apps can't download code that adds or changes features (2.5.2). Interpreted code such as JavaScript may be updated only if it doesn't change the app's primary purpose, doesn't create a store for other code and doesn't bypass the system's security (Apple Developer Program License Agreement) | Apps may update themselves only through Google Play, except code running in a JavaScript engine or WebView (Device and Network Abuse policy) | Phase 2 and `/store-release` |
| User-generated content | Filtering, reporting, blocking abusive users, and published contact information (1.2) | User Generated Content policy | Phase 2 |
| Metadata | Accurate; no names, icons or images of other mobile platforms in the app or its listing (2.3.10) | Accurate; no misleading claims | Phase 8 |
| Age rating | Updated age rating questionnaire, answers required since 31 January 2026 | IARC content rating questionnaire, target audience and the Families policy if children may use the app | Phase 7 |
| EU distribution | Digital Services Act trader status; apps without it were removed from the EU App Store from 17 February 2025 | Trader status declaration too | Phase 8 |

## Deadlines that come back every year
- **Google Play target API level.** Since 31 August 2026, new apps and updates must target Android 16 (API level 36); an extension to 1 November 2026 can be requested in Play Console. Existing apps that target lower than Android 15 stop reaching new users on newer phones. The level rises every August.
- **Apple's Xcode and SDK minimum.** Since 28 April 2026, uploads must be built with Xcode 26 and the iOS 26 SDK. A new minimum usually arrives each spring, after the September releases.
- **Memory page size on Android.** Google Play requires apps that target Android 15 or later to support 16 KB memory pages, which mostly matters for native code and is handled by current framework versions.
- **Framework upgrades.** React Native, Expo and Flutter track these deadlines in their releases, so falling more than a release or two behind turns a yearly deadline into an emergency. Phase 8 sets reminders; give each upgrade its own Issue.

## Ireland and the EU
The kit assumes the business is established in Ireland. What that adds on top of the store rules (Phase 7 checks each one against the official text):
- **The GDPR**, with Ireland's Data Protection Act 2018, supervised by the Data Protection Commission. The store forms describe the app; they don't replace the privacy notice, the legal basis for each piece of data, the records of processing, or the 72-hour breach notice to the DPC in SECURITY.md.
- **Consent on the phone.** Ireland's ePrivacy Regulations (S.I. 336/2011) require consent before an app stores or reads anything on the device that it doesn't strictly need, which covers most analytics and advertising SDKs, not only cookies on the site.
- **Children.** Under-16s can't consent to online services themselves in Ireland, and the DPC's Fundamentals apply to any service children are likely to use. Apple's Declared Age Range API and Google Play's Age Signals API are available as tools; GDPR Art. 8 and, for online platforms, the Digital Services Act decide when the app needs them.
- **Digital Services Act trader status.** As an EU business you declare trader status in App Store Connect and Play Console, and both stores then show the company's address, phone number and e-mail on the listing in the EU.
- **Cyber Resilience Act.** An app on the EU market is a product with digital elements. Since 11 September 2026, actively exploited vulnerabilities and severe incidents must be reported through ENISA's Single Reporting Platform, starting with an early warning within 24 hours. The rest (security by design, vulnerability handling, an SBOM, a stated support period) applies from 11 December 2027.
- **Product liability.** Under the new Product Liability Directive, software is a product for app versions placed on the market from 9 December 2026, and a defect caused by a missing security update can make the business liable.
- **Consumer law.** For anything sold to consumers: clear prices and subscription terms, the 14-day right of withdrawal, and, since 19 June 2026, an online withdrawal function for contracts made through the app or the site (Consumer Rights Act 2022, Directive (EU) 2023/2673). Who the trader is when Apple or Google take the payment is a question for the lawyer in Phase 7.
- **European Accessibility Act.** Since 28 June 2025 (in Ireland, S.I. 636/2023) it covers e-commerce and other consumer services, apps included, except for microenterprises providing services (fewer than 10 people and €2 million turnover or balance sheet or less). The kit's WCAG 2.2 AA target covers its technical standard.
- **AI Act.** If the app has AI features, its transparency duties apply since 2 August 2026: people are told when they talk to an AI, and AI-generated content is marked.
- **Alternative distribution and payments.** Under the Digital Markets Act, Apple allows other app marketplaces and web distribution in the EU for eligible developers, and both stores offer other ways to take payment for digital goods. Each comes with its own fees and terms that change often; compare them in Phase 2 only if the app sells digital goods.
- **VAT.** On sales made through the stores, Apple and Google charge and pay the VAT; sales through your own payment provider are yours to handle. Confirm with an accountant.
- **Android developer verification** starts in Brazil, Indonesia, Singapore and Thailand (30 September 2026) and reaches the rest of the world, Ireland included, in 2027. From then, an APK distributed by link or device management needs its developer registered. Google registers almost all apps on Google Play automatically; there is a free limited-distribution account for up to 20 devices.

## Other countries with users
Every country with users adds its own rules on top of the GDPR. SPEC.md lists the countries; Phase 7 checks each one. Two that come up often:
- **United Kingdom**, Northern Ireland included: the UK GDPR and the UK's Data Protection Act 2018, PECR for cookies, device storage and marketing, and the ICO as regulator, with a 72-hour breach notice.
- **Brazil**: the LGPD, with breach notice to the ANPD within 3 business days. The ECA Digital (Lei 15.211/2025, in force since 17 March 2026) makes app stores and operating systems provide an age signal apps can read; Apple blocks apps rated 18+ in Brazil for people not confirmed as adults, and an app with loot boxes is rated 18+ there. Android developer verification applies in Brazil since 30 September 2026.

## Releases
- **Review time.** Apple reviews most submissions within a day or two and accepts requests for an expedited review of a critical fix. Google can take from hours to several days, longer for a new app or account. Plan releases around it; a fix is not live when it merges.
- **Build numbers** must be higher than every build ever uploaded, on both stores.
- **A store build can't be rolled back.** It can only be paused and replaced by a newer one. That is why the kit has a server-side switch to turn features off, a forced-update check, and staged rollouts.
- **Staged rollouts.** Apple's phased release reaches all automatic-update users over 7 days and can be paused; Google's staged rollout goes to a percentage you choose and can be halted. Pausing stops new installs; people who already updated keep that version.
- **Listing limits** (the usual numbers; Phase 8 checks the current ones): App Store name 30 characters, subtitle 30, promotional text 170, description 4,000, keywords 100; icon 1024×1024 px. Google Play title 30 characters, short description 80, full description 4,000; icon 512×512 px; feature graphic 1024×500 px. Screenshots at the sizes each store currently asks for, taken from the real app.
