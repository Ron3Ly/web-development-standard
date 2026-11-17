---
name: implement-page
description: Implement a website page following the standard React/Vinext/Cloudflare architecture and existing project conventions.
---

# Implement Page
1. Inspect the active project's code, `AGENTS.md`, lockfile, brief, tech-stack docs and `docs/design/ui-guidelines.md` (30-item standard) when available. Read `docs/design/page-navigation.md`, `docs/architecture/routing-navigation.md` and `docs/workflows/navigation-review.md` for page/route work. If implementing data collection, analytics, account management or privacy UI, first read `docs/legal/privacy-policy-standard.md` and the approved project's data inventory.
2. Check compatibility of each needed Vinext/Cloudflare feature before coding.
3. Reuse components and design tokens. Prefer RSC unless interaction requires client code. Read `docs/design/typography.md` and implement the selected font loading, Japanese/Latin fallback and tokens (see `examples/typography.css`); validate Vinext font support instead of assuming Next.js equivalence. If Astryx is chosen by the project, read `docs/design/astryx.md`, use the pinned CLI for component APIs, and validate Vinext/RSC integration before changes.
4. Build crawlable internal links, semantic navigation and verified Vinext routing; retain browser history and query state. Define loading/error/404/auth return behavior, update title/focus appropriately after client transitions, and only add progressive/reduced-motion-safe animations if requested.
5. Add only the necessary APIs, bindings, dependencies and permissions.
6. Protect credentials; ensure data authorization, allowlisted redirects and safe error/loading states. Confirm SPA page-view tracking setup does not duplicate GA4 events. Verify third-party scripts/data transfers and data deletion paths against the approved inventory; never present template privacy wording as legally reviewed.
7. Check applicable guideline items: selection/keyboard interactions (1–16), compatibility (17–19), speed and concurrency (20–23), server-side auth and data protection (24–26), and tests/monitoring (27–30). Verify real font requests, Japanese glyphs, missing-font fallback, responsive line wraps and CLS. Run applicable lint, typecheck, tests and build from actual package scripts. Exercise the navigation test matrix on the actual app.
8. Report what changed, what was checked and what remains unverified.

## Security before code changes

Read `docs/security/security-standard.md` and `docs/security/cloudflare-hardening.md`. Check current RSC, React, Vinext and Vite plugin advisories against the resolved lockfile. Design server-side permission checks, D1 parameterization, safe R2 object access, outbound fetch allowlists, cookie/CSRF controls and secure response headers as applicable. Before finishing, execute relevant SEC-01–SEC-36 test cases in `docs/workflows/security-review.md`; state exactly which checks were actually run and what remains unverified.