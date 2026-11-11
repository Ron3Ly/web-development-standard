---
name: ui-review
description: Review implemented pages for design consistency, responsiveness, usability, accessibility, motion and UI asset licensing.
---

# UI Review
1. Read `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, `docs/design/ui-assets.md`, and the active brief.
2. Review actual rendered pages if preview/browser tools are available; otherwise state limitations.
3. Check mobile/tablet/desktop layout, content overflow and Japanese typography.
4. Check focus states, keyboard access, contrast, labels, form errors and reduced motion against applicable WCAG 2.2 Level AA success criteria. Review applicable ARIA/APG interaction patterns (roles/states/properties and keyboard behavior), preferring semantic HTML.
5. Check usability heuristics (NN/g), Japanese UI guidance (Digital Agency), context-relevant Microsoft Windows / Carbon patterns, token consistency, component reuse and per-asset licenses.
6. Classify findings by severity; provide file locations and actionable fixes.
7. Recheck fixes. Distinguish automated findings from manual keyboard/screen-reader checks; report untested WCAG criteria and do not assert conformance without evidence.
