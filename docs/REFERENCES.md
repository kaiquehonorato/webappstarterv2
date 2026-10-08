# References

The rules in this kit come from these sources. When in doubt, the original source wins. Checked in October 2026.

## Working with Claude Code (Anthropic, official)
- Best practices for Claude Code: verification, explore → plan → code, CLAUDE.md, the interview → spec pattern, context management, writer/reviewer, common failure patterns
  https://code.claude.com/docs/en/best-practices
- Model configuration: aliases, `opusplan`, effort levels, `ultrathink`, Fable
  https://code.claude.com/docs/en/model-config
- Choosing a Claude model and effort level in Claude Code (blog)
  https://claude.com/blog/claude-model-and-effort-level-in-claude-code
- Manage costs effectively / reduce token usage: https://code.claude.com/docs/en/costs
- How Claude Code uses prompt caching (what a model switch costs): https://code.claude.com/docs/en/prompt-caching
- The advisor tool: https://code.claude.com/docs/en/advisor
- Context window and auto-compaction (`autoCompactWindow`): https://code.claude.com/docs/en/context-window
- Tool output limits (what Claude sees of a long command's output): https://code.claude.com/docs/en/tools-reference#output-limits
- Output styles (Concise): https://code.claude.com/docs/en/output-styles
- Code intelligence plugins (`typescript-lsp` and others): https://code.claude.com/docs/en/plugins/code-intelligence
- Pricing (input, output and cache rates): https://platform.claude.com/docs/en/about-claude/pricing
- Memory, AGENTS.md and path-scoped rules: https://code.claude.com/docs/en/memory
- Skills (bundled `/verify`, `/run`, frontmatter reference): https://code.claude.com/docs/en/skills
- Commands (`/design`, `/code-review`, `/security-review`, `/goal`, `/deep-research`…; `/verify` and `/deep-research` run only when you type them): https://code.claude.com/docs/en/commands
- Permissions and `ask`/`deny` rules: https://code.claude.com/docs/en/permissions
- Sandboxing (an OS-enforced boundary for shell commands): https://code.claude.com/docs/en/sandboxing
- Install and manage plugins (user, project and local scope): https://code.claude.com/docs/en/discover-plugins
- Plugin loading reference (project-enabled plugins, auto-update): https://code.claude.com/docs/en/plugins/loading
- Cloud environments (what carries over into a browser session): https://code.claude.com/docs/en/cloud-environments
- Hooks: https://code.claude.com/docs/en/hooks-guide
- Hooks reference (Stop hook input, `stop_hook_active`, exit code 2, timeouts): https://code.claude.com/docs/en/hooks
- Keep Claude working toward a goal: https://code.claude.com/docs/en/goal
- Routines (scheduled cloud jobs): https://code.claude.com/docs/en/routines
- Security guidance plugin: https://code.claude.com/docs/en/security-guidance
- Claude Security plugin: https://code.claude.com/docs/en/claude-security
- Code Review: https://code.claude.com/docs/en/code-review
- Anthropic's plugin marketplaces: https://code.claude.com/docs/en/plugins/anthropic-marketplaces
- Plugin security and trust: https://code.claude.com/docs/en/plugins/security
- Official plugins catalog: https://github.com/anthropics/claude-plugins-official
- Anthropic skills: https://github.com/anthropics/skills
- Frontend design skill, copied into .claude/skills/ (Apache-2.0): https://github.com/anthropics/skills/tree/main/skills/frontend-design
- Superpowers (Jesse Vincent, MIT), the source of the two process skills in .claude/skills/: https://github.com/obra/superpowers

## Prompting (Anthropic, official)
- Prompting best practices: be clear and direct, explain why, avoid overengineering, don't hardcode to pass tests, frontend aesthetics
  https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
- Prompting Claude Opus 5.5: effort calibration, naming the design defaults to avoid, marking pasted text
  https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5

## From vibe coding to agentic engineering
- Erik Schluntz (Anthropic), "Vibe coding in prod, responsibly": focus AI-written code on leaf nodes, review the core, build verifiable checkpoints
  https://x.com/ErikSchluntz/status/1951010061871968470
- Simon Willison, Agentic Engineering Patterns: https://simonwillison.net/guides/agentic-engineering-patterns/
- Karpathy's shift from "vibe coding" to "agentic engineering" (Forbes, June 2026):
  https://www.forbes.com/sites/jodiecook/2026/06/12/is-vibe-coding-already-dead-even-karpathy-is-moving-on/
- OpenSSF, Security-Focused Guide for AI Code Assistant Instructions
  https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions.html
- AGENTS.md open standard: https://agents.md
- GitHub, Spec Kit (MIT), spec-driven development with an agent: https://github.com/github/spec-kit
  Reviewed in October 2026. Its `specify` CLI, Python and uv toolchain, `.specify/` scaffolding and separate
  constitution file are not used here, and its "tests are optional" rule contradicts this kit. Two ideas were
  adopted and rewritten in this kit's own terms: marking an unresolved decision inline where it is missing
  (Phase 0), and checking the documents against each other before building (Phase 2, item 4) and against the
  code later (Phase 9, category 10).

## Evidence about AI-written code
- Veracode, 2026 GenAI Code Security Report: https://www.veracode.com/blog/2026-genai-code-security-report-ai-risk/
- CodeRabbit, State of AI vs Human Code Generation (Dec 2025): https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report
- METR, Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity:
  https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
- Spracklen et al., "We Have a Package for You!" (package hallucinations, USENIX Security 2025):
  https://www.usenix.org/conference/usenixsecurity25/presentation/spracklen

## Incidents referenced in docs/vibe-coding-mistakes.md
- Moltbook database exposure (Wiz, Jan 2026): https://www.wiz.io/blog/exposed-moltbook-database-reveals-millions-of-api-keys
- Lovable CVE-2025-48757 (missing row-level security): https://mattpalmer.io/posts/2025/05/CVE-2025-48757/
- Tea app breach (Jul 2025): https://www.security.org/identity-theft/breach/tea-app/
- Replit agent deletes a production database (Jul 2025): https://www.business-standard.com/technology/tech-news/ai-goes-rogue-replit-ai-platform-wipes-company-database-during-code-freeze-125072200657_1.html
- Next.js middleware bypass CVE-2025-29927: https://github.com/advisories/GHSA-f82v-jwr5-mffw
- Vibe-coded apps leaking data at scale (Axios, May 2026): https://www.axios.com/2026/05/07/loveable-replit-vibe-coding-privacy
- Plugin4Shell (Sept 2026): https://thehackernews.com/2026/09/plugin4shell-lets-repository-owners.html
- Hard-coded AWS credentials in 1,859 Android and iOS apps (Symantec, Sept 2022): https://thehackernews.com/2022/09/over-1800-android-and-ios-apps-found.html

## Engineering practice
- Kent Beck, *Test-Driven Development: By Example* (red → green → refactor)
- Martin Fowler, MonolithFirst: https://martinfowler.com/bliki/MonolithFirst.html
- The Twelve-Factor App: https://12factor.net
- Google, *Site Reliability Engineering* (free online): https://sre.google/books/
- Michael Nygard, Architecture Decision Records: https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- C4 model for architecture diagrams: https://c4model.com
- Conventional Commits: https://www.conventionalcommits.org
- Martin Fowler, *Refactoring* (2nd edition): https://martinfowler.com/books/refactoring.html
- Michael Feathers, *Working Effectively with Legacy Code* (characterization tests before changing untested code)
- Knip (unused files, exports and dependencies): https://github.com/webpro-nl/knip
- jscpd (duplicate code): https://github.com/kucherenko/jscpd
- dependency-cruiser (orphans, cycles, architecture rules): https://github.com/sverweij/dependency-cruiser
- Vulture (dead Python code): https://github.com/jendrikseipp/vulture

## App stores (Apple and Google, official)
Checked on 6 October 2026. Store rules change often; when this list and the store disagree, the store wins.
- Apple Developer Program (membership, organization enrollment, D-U-N-S): https://developer.apple.com/programs/
- App Review Guidelines (4.2 minimum functionality, 4.8 login services, 5.1.1 privacy, account deletion, 3.1 payments, 2.5.2 code): https://developer.apple.com/app-store/review/guidelines/
- Upcoming requirements (Xcode and SDK minimums, privacy manifests, age ratings, EU trader status): https://developer.apple.com/news/upcoming-requirements/
- Offering account deletion in your app: https://developer.apple.com/support/offering-account-deletion-in-your-app/
- App privacy details on the App Store: https://developer.apple.com/app-store/app-privacy-details/
- Privacy manifest files: https://developer.apple.com/documentation/bundleresources/privacy-manifest-files
- App Tracking Transparency: https://developer.apple.com/documentation/apptrackingtransparency
- Age requirements for apps distributed in Brazil and other regions (Feb 2026): https://developer.apple.com/news/?id=f5zj08ey
- Declared Age Range API: https://developer.apple.com/documentation/declaredagerange/
- TestFlight: https://developer.apple.com/testflight/
- Custom apps (private distribution through Apple Business Manager): https://developer.apple.com/custom-apps/
- App Store Connect Help: https://developer.apple.com/help/app-store-connect/
- Google Play Console: https://play.google.com/console/about/
- Google Play Developer Policy Center: https://play.google.com/about/developer-content-policy/
- Target API level requirements for Google Play: https://developer.android.com/google/play/requirements/target-sdk
- App testing requirements for new personal developer accounts (12 testers, 14 days): https://support.google.com/googleplay/android-developer/answer/14151465
- Account deletion requirements on Google Play: https://support.google.com/googleplay/android-developer/answer/13327111
- Data safety section on Google Play: https://support.google.com/googleplay/android-developer/answer/10787469
- Play App Signing: https://support.google.com/googleplay/android-developer/answer/9842756
- Play Age Signals API (beta): https://developer.android.com/google/play/age-signals/use-age-signals-api
- Android developer verification (Brazil from 30 September 2026): https://developer.android.com/developer-verification
- Android vitals (crash, ANR and startup thresholds): https://developer.android.com/topic/performance/vitals

## Building phone apps
- Apple Human Interface Guidelines: https://developer.apple.com/design/human-interface-guidelines/
- Material Design 3: https://m3.material.io
- Android app quality guidelines: https://developer.android.com/quality
- Apple accessibility for developers: https://developer.apple.com/accessibility/
- Android accessibility: https://developer.android.com/guide/topics/ui/accessibility
- W3C, mobile accessibility and WCAG: https://www.w3.org/WAI/standards-guidelines/mobile/
- React Native: https://reactnative.dev · Expo: https://docs.expo.dev · Flutter: https://docs.flutter.dev · Capacitor: https://capacitorjs.com
- Maestro (end-to-end flows on simulators and devices, Apache-2.0): https://github.com/mobile-dev-inc/maestro
- Detox (end-to-end testing for React Native, MIT): https://github.com/wix/Detox
- React Native Testing Library: https://github.com/callstack/react-native-testing-library
- RFC 8252, OAuth 2.0 for Native Apps (system browser, PKCE): https://www.rfc-editor.org/rfc/rfc8252
- MobSF, Mobile Security Framework (GPL-3.0, run as a tool): https://github.com/MobSF/Mobile-Security-Framework-MobSF

## Security
- OWASP MASVS (Mobile Application Security Verification Standard) and MASTG (testing guide): https://mas.owasp.org
- OWASP Mobile Top 10 (2024): https://owasp.org/www-project-mobile-top-10/
- OWASP Top 10:2025: https://owasp.org/Top10/
- OWASP API Security Top 10: https://owasp.org/API-Security/
- OWASP ASVS 5.0: https://owasp.org/www-project-application-security-verification-standard/
- OWASP GenAI Security Project (Top 10 for LLM Applications 2025, Top 10 for Agentic Applications 2026): https://genai.owasp.org
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org
  - The ones behind the new audit checks: Cross-Site Request Forgery Prevention, Forgot Password, Session Management, File Upload, HTTP Headers, Logging, Secrets Management, Authorization, Insecure Direct Object Reference Prevention, LLM Prompt Injection Prevention
- Supabase, API keys (publishable versus secret keys, what bypasses RLS): https://supabase.com/docs/guides/api/api-keys
- Firebase Security Rules: https://firebase.google.com/docs/rules
- Amazon S3 Block Public Access: https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html
- GitHub secret scanning push protection: https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection
- Semgrep Community Edition (static analysis on any plan): https://github.com/semgrep/semgrep
- gitleaks (secret scanning): https://github.com/gitleaks/gitleaks
- OWASP ZAP (DAST): https://www.zaproxy.org

## Repository, supply chain and backups
- GitHub, protected branches and rulesets: private repositories need GitHub Pro, Team or Enterprise
  https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
  https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets
- GitHub, environments and environment secrets (Pro or higher for private repositories): https://docs.github.com/en/actions/deployment/targeting-different-environments/managing-environments-for-deployment
- GitHub, where secret scanning and code scanning are available (not on a personal account's private repositories): https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning and https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning
- GitHub plans and prices: https://github.com/pricing
- pnpm settings (`minimumReleaseAge`, `strictDepBuilds`, `allowBuilds`, `trustPolicy`): https://pnpm.io/settings
- Renovate, the `config:best-practices` preset: https://docs.renovatebot.com/presets-config/#configbest-practices
- Dependabot options, including `cooldown`: https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference
- Supabase, database backups (plans, point-in-time recovery, Storage files not included): https://supabase.com/docs/guides/platform/backups
- Playwright, screenshot comparisons and the Docker image: https://playwright.dev/docs/test-snapshots and https://playwright.dev/docs/docker
- size-limit (JavaScript size budget in CI): https://github.com/ai/size-limit
- Lighthouse CI: https://github.com/GoogleChrome/lighthouse-ci

## Law and compliance (Ireland and the EU)
Checked on 6 October 2026. The kit assumes a business established in Ireland.
- GDPR, Regulation (EU) 2016/679: https://eur-lex.europa.eu/eli/reg/2016/679/oj
- Data Protection Act 2018 (Ireland; s. 31 sets the digital age of consent at 16): https://www.irishstatutebook.ie/eli/2018/act/7/enacted/en/html
- Data Protection Commission (guidance, DPO notification, the list of processing that needs a DPIA): https://www.dataprotection.ie
- DPC breach notification form: https://forms.dataprotection.ie/breach-notification
- DPC, Fundamentals for a Child-Oriented Approach to Data Processing: https://www.dataprotection.ie/en/dpc-guidance/fundamentals-child-oriented-approach-data-processing
- ePrivacy Regulations, S.I. No. 336 of 2011 (consent for storing or reading information on a device; electronic marketing): https://www.irishstatutebook.ie/eli/2011/si/336/made/en/print
- Standard contractual clauses for transfers outside the EEA, Decision (EU) 2021/914: https://eur-lex.europa.eu/eli/dec_impl/2021/914/oj/eng
- EU-US Data Privacy Framework (upheld by the General Court in September 2025, appeal pending at the Court of Justice as of October 2026): https://www.dataprivacyframework.gov
- Cyber Resilience Act, Regulation (EU) 2024/2847: https://eur-lex.europa.eu/eli/reg/2024/2847/oj
  - Its reporting obligations, in force since 11 September 2026: https://digital-strategy.ec.europa.eu/en/policies/cra-reporting
- Product Liability Directive (EU) 2024/2853 (software is a product, from 9 December 2026): https://eur-lex.europa.eu/eli/dir/2024/2853/oj
- AI Act, Regulation (EU) 2024/1689: https://eur-lex.europa.eu/eli/reg/2024/1689/oj
  - Guidelines on the Art. 50 transparency obligations, in force since 2 August 2026: https://digital-strategy.ec.europa.eu/en/policies/guidelines-ai-transparency-obligations
- Digital Services Act, Regulation (EU) 2022/2065: https://eur-lex.europa.eu/eli/reg/2022/2065/oj
- Coimisiún na Meán (Ireland's Digital Services Coordinator, Online Safety Code): https://www.cnam.ie
- Consumer Rights Act 2022 (No. 37 of 2022): https://www.irishstatutebook.ie/eli/2022/act/37/enacted/en/html
- Consumer Protection Act 2007 (No. 19 of 2007; unfair and misleading commercial practices): https://www.irishstatutebook.ie/eli/2007/act/19/enacted/en/html
- The online withdrawal function, Directive (EU) 2023/2673 (applies since 19 June 2026): https://eur-lex.europa.eu/eli/dir/2023/2673/oj
- Competition and Consumer Protection Commission (CCPC): https://www.ccpc.ie
- E-Commerce Regulations, S.I. No. 68 of 2003 (information an online service must show): https://www.irishstatutebook.ie/eli/2003/si/68/made/en/print
- European Accessibility Act, Directive (EU) 2019/882: https://eur-lex.europa.eu/eli/dir/2019/882/oj
- European Union (Accessibility Requirements of Products and Services) Regulations 2023, S.I. No. 636 of 2023: https://www.irishstatutebook.ie/eli/2023/si/636/made/en/print
- Equal Status Act 2000: https://www.irishstatutebook.ie/eli/2000/act/8/enacted/en/html
- EUIPO (EU trade marks): https://www.euipo.europa.eu
- Intellectual Property Office of Ireland: https://www.ipoi.gov.ie
- Choose a License: https://choosealicense.com

### Users in other countries
- United Kingdom: the Information Commissioner's Office (UK GDPR, PECR): https://ico.org.uk
- Brazil:
  - LGPD, Lei 13.709/2018: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm
  - ANPD, security incident communication (Res. CD/ANPD 15/2024, 3 business days): https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/comunicado-de-incidente-de-seguranca-cis
  - ANPD, international data transfers (Res. CD/ANPD 19/2024): https://www.gov.br/anpd/pt-br/assuntos/assuntos-internacionais/transferencia-internacional-de-dados
  - ECA Digital, Lei 15.211/2025 (in force since 17 March 2026): https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2025/lei/l15211.htm
  - Marco Civil da Internet, Lei 12.965/2014: https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm
  - Código de Defesa do Consumidor, Lei 8.078/1990: https://www.planalto.gov.br/ccivil_03/leis/l8078compilado.htm
  - E-commerce rules, Decreto 7.962/2013: https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2013/decreto/d7962.htm
  - Lei Brasileira de Inclusão, Lei 13.146/2015: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2015/lei/l13146.htm
  - Lei de Direitos Autorais, Lei 9.610/1998: https://www.planalto.gov.br/ccivil_03/leis/l9610.htm
  - AI bill PL 2338/2023 (approved by the Senate in Dec 2024, pending in the Chamber as of Oct 2026): https://www25.senado.leg.br/web/atividade/materias/-/materia/157233

## UX, accessibility, motion and performance
- WCAG 2.2: https://www.w3.org/TR/WCAG22/
- Core Web Vitals: https://web.dev/articles/vitals
- Material Design, motion: https://m3.material.io/styles/motion/overview
- Apple Human Interface Guidelines, motion: https://developer.apple.com/design/human-interface-guidelines/motion
- Nielsen Norman Group, response time limits: https://www.nngroup.com/articles/response-times-3-important-limits/
- React Three Fiber, scaling performance (on-demand rendering): https://r3f.docs.pmnd.rs/advanced/scaling-performance
- View Transitions API browser support: https://caniuse.com/view-transitions-api
- GSAP license change (free, with a custom license): https://discourse.webflow.com/t/webflow-makes-gsap-100-free/319967
