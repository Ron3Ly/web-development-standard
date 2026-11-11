---
name: ui-review
description: Review implemented pages for design consistency, responsiveness, usability, accessibility, motion and UI asset licensing.
---

# UI Review
1. Read `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, `docs/design/ui-assets.md`, and the active brief.
2. Review actual rendered pages if preview/browser tools are available; otherwise state limitations.
3. Check mobile/tablet/desktop layout, content overflow and Japanese typography.
4. Review applicable numbered source recommendations No.1–19, including row/bulk selection, feedback, navigation, forms, keyboard controls, contrast, locale and browser behavior. Treat WCAG 2.2 AA as a target candidate unless the project mandates it; use APG for custom widgets.
5. Review applicable No.20–30 for Core Web Vitals (LCP/INP/CLS), large datasets, failures/concurrent edits, authentication/authorization, OWASP risks, privacy, E2E tests, monitoring and maintainability. Check token consistency, component reuse and asset licenses.
6. Classify findings by severity; provide file locations and actionable fixes.
7. Recheck fixes. Record applicable item numbers, evidence and not-tested items; distinguish automated findings from manual keyboard/screen-reader checks. Do not assert conformance without evidence.
