---
name: design-page
description: Plan a new page using this repository's design standards and asset references. Use for UI architecture, page layouts, and redesign planning.
---

# Design Page
1. Read `AGENTS.md`, `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, `docs/design/typography.md`, **`docs/design/page-navigation.md`**, `docs/design/ui-references.md`, `docs/design/ui-assets.md`, and the project brief. For privacy policy/account/analytics UI, read `docs/legal/privacy-policy-standard.md`.
2. State target users, goals, content, constraints and accessibility requirements.
3. Propose information hierarchy, page sections, reusable component inventory and responsive behavior. Create a route map and primary user-journey flows; define link labels, breadcrumbs/active states, URL state, back/forward and focus/scroll expectations.
4. Map the page to applicable numbered items from the six-category/30-item guideline. Detail No.1–14 and No.15–16 first as the source recommends; include relevant items No.17–30 when the feature requires them. For accessibility, confirm whether WCAG 2.2 AA is the project's target.
5. Name the relevant design reference principles and asset candidates; flag license checks. Consider Astryx components/templates as an **optional** implementation candidate and consult `docs/design/astryx.md` before suggesting it.
6. Select the actual font preset and role-based type scale; default to Geist Sans + Noto Sans JP, Geist Mono for code. Use a different researched font only when the target audience and branding justify it. Account for Japanese body text and loading performance.
7. Record assumptions, project-specific overrides and open questions.
8. Produce a concise design specification suitable for implementation, including a navigation/transition matrix and loading/error/unsaved-data states. Optional animation is off by default and must support reduced motion. Provide discoverable, accessible privacy-policy links at appropriate points in the user journey and never use a consent interface that assumes consent without a meaningful user action when consent is legally required.

Do not copy protected visual assets or adopt an unrelated technology stack.

## Security during design

Before planning authenticated or data-backed pages, read `docs/security/security-standard.md` and identify route/role/data boundaries (SEC-01–SEC-12). Capture the threat-model assumptions, privacy requirements and high-risk user journeys in the project brief. Do not treat hiding controls in the UI as authorization.
## Quality design

Read `docs/quality/quality-standard.md` and the project-specific quality target. Identify tasks, interface states, real Japanese content, accessibility, browser support, expected loading/errors and public SEO requirements. Use Q-01–Q-18 and applicable Q-25–Q-30. A screenshot-only review is not functional acceptance.