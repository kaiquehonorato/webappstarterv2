# Security Policy

## Reporting a vulnerability
E-mail: <security@your-domain> (do NOT open a public issue or a store review). We reply within 2 business days. The same address appears on the support page that both store listings link to.

How the app and its backend are protected and how they test themselves: [docs/security-model.md](docs/security-model.md).

## Supported versions
A store app can't be patched in place: every installed copy stays on its version until the person updates it. So:
- The backend knows the **minimum supported app version**. Below it, the app shows an update screen with a link to the store instead of running.
- A security fix ships as a store release with an expedited review if needed, then the minimum version goes up once the fix is approved and available in both stores.
- Versions older than <N months or the last N releases> are not supported. Fill this in at Phase 8. From December 2027 the Cyber Resilience Act also requires a stated support period with free security updates; Phase 7 sets it.

## Incident response plan (personal data and security)
1. **Contain**: revoke leaked keys and tokens; block the attack path on the server (the server is the only place you control right away); halt any staged rollout or over-the-air update that carries the problem; preserve logs (don't delete evidence).
2. **Assess**: what data, how many people, since when, is it still exposed? Which app versions are affected? Is the breach likely to result in a risk, or a high risk, to the people affected?
3. **Notify under the GDPR** (Art. 33 and 34, with Ireland's Data Protection Act 2018):
   - The **Data Protection Commission** without undue delay and within **72 hours** of becoming aware of a personal data breach, unless it is unlikely to result in a risk to people. Use the DPC's breach notification form: https://forms.dataprotection.ie/breach-notification. If not everything is known yet, notify what is known and send the rest as it arrives.
   - **Affected people** without undue delay when the breach is likely to result in a high risk to them: in plain language, what happened, what it means for them and what they can do.
   - Users in other countries can add their own deadlines: Brazil's ANPD expects notice within 3 business days (Resolution CD/ANPD 15/2024), the UK's ICO within 72 hours.
4. **Report under the Cyber Resilience Act** (since 11 September 2026), when an actively exploited vulnerability or a severe incident affects the security of the app: an early warning within **24 hours** of becoming aware, a notification within 72 hours, and a final report (within 14 days of a fix being available for a vulnerability, within one month of the notification for an incident), through ENISA's Single Reporting Platform, which routes it to Ireland's national CSIRT. Tell affected users, and how they can protect themselves, when that helps.
5. **Fix + post-mortem**: root cause, fix, regression test, a store release or a signed over-the-air update, the minimum supported version raised when the fix is live, and a GitHub Issue for each follow-up.
6. **Record**: document every personal data breach, its effects and what was done about it, even when notification wasn't required (GDPR Art. 33(5)).

## If a signing key or a store account is compromised
- **Android upload key**: with Play App Signing, ask Google to reset the upload key from Play Console; Google keeps the app signing key. Without Play App Signing a lost key means the app can never be updated again, which is why the kit turns it on in Phase 3.
- **Apple certificates and push keys**: revoke them in the Apple Developer account and create new ones; existing installs keep working.
- **Store account**: change the password, check two-step sign-in and the list of users and roles, and look at what changed in App Store Connect or Play Console since the compromise.

Data protection contact: <name, contact>. A Data Protection Officer is required only in the cases in GDPR Art. 37 (for example, large-scale monitoring of people or large-scale processing of health data); if one is appointed, publish their contact and tell the DPC.

> This plan is a technical starting point, not legal advice. Have it reviewed by a lawyer.
