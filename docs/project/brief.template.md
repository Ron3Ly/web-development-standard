# Project Brief (copy into each project repository)

## Project identity
- Name:
- Purpose / key user problem:
- Target audience:
- Key user journeys / calls to action:
- Content and language:

## Pages and features
- Sitemap / route inventory (public, protected, auth, error):
- Primary navigation / information hierarchy and active-location cues:
- User journey diagrams (list -> detail -> edit -> back, registration etc.):
- Route URL / query state contract (filter/sort/page, direct links):
- Browser history and scroll restoration rules:
- Route-change focus, page title and assistive technology announcements:
- Loading/empty/error/not-found/permission-denied navigation states:
- Leave-with-unsaved-changes policy:
- Transition animation policy (none by default; reduced motion and feature support):
- Required pages:
- Forms and interactions:
- Responsive priorities:
- Accessibility / legal requirements (WCAG 2.2 AA is a candidate; confirm target and scope):

## Technical choices
- JavaScript or TypeScript:
- Vinext version and verified features:
- CSS strategy / utility framework (if any):
- UI component library: Astryx (adopt / reference only / do not use), version and rationale:
- Cloudflare Workers setup:
- D1 required? Schema/migrations:
- R2 required? Storage policy:
- Google login required? Specific provider:
- Anonymous access required? Persistence/account linking:
- GA4 / Cloudflare Web Analytics required? Events and consent:
- SPA page-view tracking method, virtual page paths, duplicate-measurement prevention:
- External music providers required? Scopes / quotas / terms:

## Branding / UI
- Brand colors and logo rights:
- Font preset (default Geist Sans + Noto Sans JP / Geist Mono, or an approved alternative):
- Language/script mix and Japanese font fallback:
- Font source and delivery (first-party WOFF2 / verified Vinext font loader / provider CDN):
- Font versions/weights, OFL notices and loading-performance budget:
- Font QA: glyph coverage, numerals/tables, line wraps, zoom, fallback, CLS:
- Reference UI and specific patterns:
- Applicable guideline item numbers (No.1–30 from `docs/design/ui-guidelines.md`) and rationale:
- Required keyboard/multi-select/table patterns and WCAG/APG checks:
- Performance / large data / reliability / concurrency needs (No.20–23):
- Security and data protection requirements (No.24–26):
- E2E testing / monitoring / maintainability / UX metrics (No.27–30):
- Icon/illustration choices and license notes:
- Design system exceptions:

## 2D Web game assets (only for game projects)

- Genre, pixel/vector visual language, game area, target browser/device:
- Selected icons/UI/sprites/VFX/SFX/BGM and target file sizes:
- Asset source & license inventory (copy `docs/game-assets/asset-register.template.md`):
- Attribution/Credits UI and link (when required by each asset's license):
- Commercial/ads/paid use and source/public GitHub/R2 distribution rights:
- User-gesture audio enablement, volume/mute, non-audio cues, supported codecs:
- VFX reduced motion, flashing threshold WCAG 2.3.1, device FPS/texture budget:
- GAME-01–GAME-24 statuses and permission/licensing approver:

## Quality targets and acceptance

- Quality owner, release approver and agreed acceptance scope:
- Important user journeys and error/empty/loading states to test:
- Project quality targets (copy `docs/quality/quality-targets.template.md`):
- Browsers/OS/device/network, Japanese/English and supported Web APIs:
- Accessibility target/representative pages/manual keyboard and screen-reader checks:
- Core Web Vitals field p75 targets and actual data coverage (PSI/CrUX/RUM):
- Lighthouse CI / performance lab budget and measurement conditions:
- Public SEO/content/indexability requirements and content owner:
- Cloudflare Workers errors/latency + D1/R2 and API recovery expectations:
- Service-level indicators/SLIs, SLO period and alert/recovery owner:
- Unit/integration/Playwright E2E and visual regression coverage:
- Q-01–Q-36 quality review statuses, evidence, exclusions and risk acceptance:

## Security requirements and risk acceptance

- Security risk tier and OWASP ASVS 5.0.0 target level (L1 / L2 / L3, rationale):
- Security owner, incident contact, release approver and documented exceptions:
- Threat model location (copy `docs/security/threat-model.template.md`):
- Assets, user roles/tenants, trust boundaries, auth/session model and attack surface:
- Authentication (Google/guest) PKCE/state/nonce, cookies, CSRF, account linking:
- Authorization rules per endpoint/server action/D1 row/R2 object and negative cross-user tests:
- Input, output encoding, SQL parameterization, SSRF, upload/storage limits:
- Public/private cache, CDN, CSP/headers/CORS and third-party script requirements:
- Cloudflare secrets/bindings/environment isolation/WAF/rate-limit requirements:
- Verified React/RSC/Vinext and provider dependency versions; security advisories:
- CI permissions, secret scanning, SAST/SCA, review branch protections, deploy approval:
- Logs/alerts, backup/restore/rollback, incident and breach response process:
- Security review status (SEC-01–SEC-36 and `docs/workflows/security-review.md`):

## Privacy / personal data / policy publication
- Controller/operator legal name, physical business address, corporate representative and privacy contact (verified):
- Market / audience / applicable laws (Japan APPI, telecom external-transmission scope, EU/other when relevant):
- 2026 APPI amendment implementation-date / PPC guideline review (as of launch):
- Data inventory location (copy `docs/legal/data-inventory.template.md` and complete):
- Collected categories: Google profile, anonymous IDs, cookies, logs, D1/R2 objects, API integrations, user-generated content:
- Each data category's purposes, collection source, retention/deletion, security safeguards and request-handling procedure:
- GA4 / Cloudflare Web Analytics real scripts, external sends, required notices, consent choices and evidence:
- Google OAuth scopes and public privacy policy URL on the same domain (if applicable):
- Vendors / outsourcing / third-party sharing / overseas handling and documented assessments:
- Data subject rights, user deletion, opt-out and support contact:
- Actual public policy URL (example `/privacy`) and last updated date:
- Reviewer/approver and outstanding [NEEDS VERIFICATION] items:
- Privacy review checklist (PR-01–PR-20 in `docs/workflows/privacy-review.md`):

## Delivery / testing
- Environments / deployment:
- Target performance and accessibility:
- Security / privacy requirements:
- Security verification evidence / exceptions / risk acceptance and release sign-off:
- Privacy policy owner/legal review and provider terms checklist:
- Acceptance criteria:
- Navigation review: direct URL, refresh, back/forward, keyboard, mobile, scroll/focus, auth return, slow network, reduced motion:
- Known open questions:

## Deviations from the shared standard
| Item | Reason | Approved by / date |
| --- | --- | --- |
| | | |
