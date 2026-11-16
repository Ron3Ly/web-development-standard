---
name: ui-review
description: Review implemented pages for design consistency, responsiveness, usability, accessibility, motion and UI asset licensing.
---

# UI Review
1. Read `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, `docs/design/typography.md`, `docs/design/page-navigation.md`, `docs/architecture/routing-navigation.md`, `docs/workflows/navigation-review.md`, `docs/design/ui-assets.md`, and the active brief. For sites with user data, analytics or login, read `docs/legal/privacy-policy-standard.md` and `docs/workflows/privacy-review.md`.
2. Review actual rendered pages if preview/browser tools are available; otherwise state limitations.
3. Check mobile/tablet/desktop layout, content overflow, fonts actually loaded, correct Latin/Japanese glyph assignment, missing-glyph boxes, Japanese line wraps, table numeral alignment, 200% zoom and missing-font fallbacks.
4. Review applicable numbered source recommendations No.1–19, including row/bulk selection, feedback, navigation, forms, keyboard controls, contrast, locale and browser behavior. Treat WCAG 2.2 AA as a target candidate unless the project mandates it; use APG for custom widgets.
5. Review applicable No.20–30 for Core Web Vitals (LCP/INP/CLS), large datasets, failures/concurrent edits, authentication/authorization, OWASP risks, privacy, E2E tests, monitoring and maintainability. Check token consistency, component reuse and asset licenses.
6. Verify navigation states with actual routes: direct loads/refresh, browser back/forward, deep links and query persistence, keyboard focus, aria-current, titles/landmarks, history scroll restoration, auth loops, not-found/forbidden states, reduced motion, links/SEO and page-view analytics. Run the matrix in `docs/workflows/navigation-review.md`.
7. Classify findings by severity; provide file locations and actionable fixes.
8. Recheck fixes. Record applicable item numbers, evidence and not-tested items; distinguish automated findings from manual keyboard/screen-reader checks. Evaluate font-related layout shift, font request failures, unused fonts/weights and license notices. For applicable sites, check data inventory versus actual scripts/API calls and policy text, policy page access, Google OAuth link and external-transmission notice; report unverified items. Do not assert conformance without evidence.
