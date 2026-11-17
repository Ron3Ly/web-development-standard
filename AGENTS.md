# AI Agent Instructions — Web Development Standard

This repository is a **shared standards reference**, not an executable application. Respond in Japanese unless the project requests otherwise.

## Required reading
Before planning, designing, or implementing a website:
1. Read `docs/architecture/tech-stack.md`. **Security baseline:** before any architecture or implementation work, read `docs/security/security-standard.md`. For auth, Workers, D1/R2, API or RSC work also read `docs/security/cloudflare-hardening.md`; for launch or review read `docs/workflows/security-review.md`.
2. Read `docs/architecture/cloudflare.md` and `docs/architecture/authentication.md` when relevant.
3. Read `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, **`docs/design/typography.md`**, **`docs/design/page-navigation.md`**, `docs/design/ui-references.md`, and `docs/design/ui-assets.md`. Read `docs/design/astryx.md` when evaluating or using UI components.
4. Read **`docs/architecture/routing-navigation.md`** whenever planning or implementing routes, links, transitions, auth redirects, navigation state, or navigation analytics. Read `docs/workflows/navigation-review.md` for verification.
5. **Privacy / legal:** Read `docs/legal/privacy-policy-standard.md`, `docs/legal/data-inventory.template.md`, and `docs/workflows/privacy-review.md` when building/altering account flows, cookies, analytics, external APIs, data storage, data requests, or public-facing privacy notices. Use `docs/legal/privacy-policy.template.md` only as a draft, never a finished compliance statement.
6. Read `docs/architecture/external-integrations.md` if using music APIs.
7. Read the active project's brief. Use `docs/project/brief.template.md` if none exists.

## Quality standards — required reading

- Before setting acceptance criteria or reviewing any page, read `docs/quality/quality-standard.md`, `docs/quality/quality-targets.template.md`, and `docs/workflows/quality-review.md`; for measurements read `docs/quality/measurement-playbook.md`, and for Cloudflare reliability read `docs/quality/reliability-observability.md`.
- Apply **Q-01–Q-36** to relevant pages with actual evidence. Build and test primary user flows, including failures, anonymous→Google login, D1/R2 consistency, external music API fallback, mobile browser behavior and content quality as applicable.
- Google CWV good field thresholds: **LCP p75 ≤2.5s, INP p75 ≤200ms, CLS p75 ≤0.1**. These are distinct from Lighthouse lab scores. If field coverage is absent, use `Insufficient data` and perform lab/real-browser checks; never invent CWV measurements.
- WCAG 2.2 AA is a candidate quality target; automated accessibility audits alone do not establish conformance. Base the scope on the project and follow WCAG-EM/appropriate manual tests.
- Browser/version support follows the target audience and Web Platform Baseline. **Never impose a universal Lighthouse 90 score or 99.9% uptime SLO** without project acceptance.
- Record Q-check statuses as Pass/Fail/Not tested/Not applicable and quality risk owners. Quality does not waive SEC, privacy or navigation release requirements.

## Technical defaults
- React, Vinext, RSC where supported, Vite/Rolldown, Cloudflare Workers.
- D1 for relational storage, R2 for objects, **only when required**.
- Google login and anonymous/guest authentication are possible requirements, **not a preselected auth library**.
- GA4 and Cloudflare Web Analytics when specified and privacy requirements permit. Their data collection and external transmissions must be inventoried before publication.
- **Typography default for new Japan-facing websites:** use Geist Sans for Latin/UI numerals and Noto Sans JP for Japanese text, with OS Japanese sans-serif fallbacks; use Geist Mono for code/monospaced data only. Actually configure the fonts in the project, not merely mention them. Read `docs/design/typography.md` and use `examples/typography.css` as the starting token/stack reference. Existing projects and project briefs may override the default.
- **Alternate typography presets by context:** Noto Sans JP or BIZ UDPGothic for Japanese readability-focused content; Inter for Latin-heavy products; IBM Plex Sans JP for dense enterprise UI; Noto Serif JP for editorial titles; Zen Maru Gothic for friendly-brand titles. Do not load all candidates.
- CSS with utility classes. **Do not assume Tailwind or shadcn/ui is mandatory**.
- [Astryx](https://github.com/meta/astryx) is an **optional React 19+ UI component/design-system candidate**, not a mandatory dependency. It is in beta; verify Vinext/RSC/Vite/Cloudflare compatibility before adoption.
- JavaScript or TypeScript: the original preference is unresolved; inspect existing code and confirm for greenfield work.
- Music integrations (Spotify, Apple Music, YouTube, setlist.fm, MusicBrainz, Deezer) are **opt-in**.

## Design defaults
- Use `docs/design/ui-guidelines.md` as the **primary 30-item application-development guideline** across six areas: UX/usability (1–7), UI/information design (8–14), accessibility/environment (15–19), performance/reliability (20–23), security/data protection (24–26), and QA/operations/improvement (27–30). Apply context-relevant recommendations; the source explicitly distinguishes standards, design guidance and best practices.
- Consider WCAG 2.2 Level AA as the accessibility target candidate identified in guideline No.15; confirm the actual project requirement. Use APG patterns for keyboard/ARIA interaction guidance (No.16), preferring native semantic HTML. Do not claim conformance without verification.
- Follow the source document's prioritization: first detail No.1–14 and No.15–16 for interactive web applications; also address applicable performance, security and QA practices before delivery. Use the Digital Agency Design System for relevant Japanese-language UI patterns.
- Consider Astryx as a component implementation resource and design-system reference, not a replacement for the Digital Agency design principles. If selected, consult the pinned project's Astryx CLI/component documentation; do not invent component APIs.
- Prefer existing components and tokens, consistent responsive layout, meaningful interaction and restrained motion.
- **Page navigation:** start with a sitemap/route map and primary user journeys; use real links for navigation and buttons for actions. Preserve deep links, browser history, accessible current-page indicators, scroll and focus expectations. See `docs/design/page-navigation.md`.
- **Routing:** use version-verified Vinext App Router / `next/link` / `next/navigation`; never invent router APIs or assume Next.js behavior is identical. Prefer functional navigation without transitions; progressive motion only if useful and reduced-motion-safe. See `docs/architecture/routing-navigation.md`.
- **Security/analytics:** validate internal auth `returnTo` targets and server-side authorization; ensure meaningful loading/error states, crawlable public links, and no double-counted GA4/Cloudflare SPA page views.
- **SEC-01–SEC-36 security baseline:** establish an appropriate threat model and choose an OWASP ASVS 5.0.0 verification target (L1 for low-risk starting scope, L2 normally for authenticated/personal-data applications, L3 where risk warrants). Verify each applicable control; do not claim ASVS compliance based only on this checklist.
- **Never trust the client:** perform object-level and tenant-level authorization in server-side RSC actions, routes and D1/R2 access, for **every operation**; use D1 parameter binding, output-appropriate encoding and secure validation of external URLs/files. Use CSRF protection for cookie-authenticated mutations and defend against abuse and redirects.
- **RSC/Cloudflare security:** verify pinned React/React Server DOM, `@vitejs/plugin-rsc`, Vinext and transitive versions against current security advisories before release. Do not assume a WAF, CSP or Cloudflare automatically patches application dependencies. Keep Workers secrets out of public vars/client bundles and private R2 objects private.
- **Supply chain/operations:** protect CI/CD and deployments with least privilege, reviewed dependencies and scanning; keep an incident and rollback procedure and minimally sufficient privacy-aware security logs. Block release for unresolved critical/high-risk vulnerabilities unless a named risk owner explicitly accepts an evidenced exception per `docs/workflows/security-review.md`.
- **Privacy-by-design / public disclosures:** inventory actual personal data, identifiers, cookies, logs, Cloudflare D1/R2, Google sign-in, GA4 and optional music API flows before writing policy text. Never invent operator identity, contact details, purposes, recipients, storage regions, retention, consent mechanics, or legal compliance. Map each statement to verified evidence. Use the legal templates and review workflow; a human owner (and legal counsel when appropriate) must approve public wording.
- **Current law matters:** Japanese APPI disclosure rules, applicability of the Telecommunications Business Act external-transmission obligations, overseas recipients, EU/other region requirements, and the phased 2026 APPI amendments require a release-date check. Do not assert that every cookie universally requires consent in Japan or that a privacy policy by that title is universally mandated.
- Limit loaded font families and weights; use Japanese-specific glyph/subset optimization, WOFF2, sensible `font-display`, system fallback and font license notices. On Vinext, do not assume Next.js `next/font` self-hosting/subsetting; validate the pinned version and Cloudflare production build.
- Approved candidate asset sources: Lucide Animated, ICOOON MONO, Digital Agency illustration/icon assets, freeicon.dev.
- Check **specific asset licenses** before copying or redistributing. Respect reduced-motion preferences.
- References are inspiration, **not permission to copy branded UI**.

## Execution process
1. Inspect project requirements, repository structure, lockfile and the current dependency versions.
2. Verify Vinext compatibility and Cloudflare runtime/bindings for the required features.
3. Document assumptions and deviations before installing new frameworks/libraries.
4. Implement minimal, reusable, accessible code; protect secrets server-side.
5. Where available, run the project's lint, type check, tests and production build.
6. Run applicable **Q-01–Q-36** and **SEC-01–SEC-36** checks and threat-model review via `docs/workflows/security-review.md`, documenting Passed/Failed/Not tested/Not applicable with evidence; do not claim security certification or run destructive tests against production. Review the applicable 30-item guideline checks, including responsive behavior, selection and table workflows, WCAG/APG interactions, Core Web Vitals, server-side authorization, privacy, E2E tests and monitoring as relevant. Validate actual font loading, Japanese glyph coverage, 200% zoom, fallback and CLS. Verify navigation using `docs/workflows/navigation-review.md`: direct URLs, back/forward, filters, auth return, keyboard/screen reader focus, reduced motion, slow network, 404/error and page-view analytics. Run the relevant PR-01–PR-20 privacy checks in `docs/workflows/privacy-review.md` and report open items; do not describe an unreviewed template as a published, legally verified policy. Distinguish recommendations from mandatory requirements and report unverified checks.
7. Report changed files, quality/security check results, field-vs-lab measurement sources, remaining risks, validation, known limitations and deviations. Record owner-approved exceptions instead of silently weakening controls.

## Rules for other repositories
A `GitHub URL` to this repository **does not automatically install or activate these instructions**. Copy the relevant standard into each target repo, or explicitly make these files accessible to GPT Work / Codex and direct the agent to read them. Codex's automatic AGENTS.md discovery follows the **working repository's hierarchy**, not unrelated repositories.

## Conflicts
Security, applicable law, user requirements, accessibility and actual runtime compatibility take precedence. Record exceptions in the active project's brief. Do not treat proposals here as verified implementations.
