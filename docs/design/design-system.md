# Design System — Shared Principles

## Design source
Primary reference: [Digital Agency Design System (DADS)](https://design.digital.go.jp/dads/).

Use its guidance for accessibility, user needs, typography, spacing, components, navigation and forms as a **design reference**. It does not require copying the agency's branding or colors.

Also read the [Webアプリケーション開発ガイドライン（6分野・30項目）](ui-guidelines.md) for practical recommendations and references covering UX, UI, accessibility, performance, security and quality/operations. The attached source text is kept intact; its recommendations must be adapted to the product's audience and requirements.

## Optional implementation system: Astryx

[Astryx](https://github.com/meta/astryx) is a candidate React component system (150+ components, theme tokens, templates and agent-oriented CLI). It complements — **does not supersede** — the Digital Agency Design System's principles. It is beta and requires React 19+; do not automatically install it. If the project chooses Astryx, use its documented component APIs, themes and tokens rather than rebuilding existing primitives. See [Astryx integration notes](astryx.md).

## Visual defaults
- Readable hierarchy, clear navigation, consistent components and generous but purposeful whitespace.
- Geist is the preferred Latin font; select a legible Japanese fallback (e.g., system Japanese sans-serif) and test mixed scripts.
- Centralize colors, spacing, type scales, border radii and interaction states in project design tokens.
- Avoid arbitrary one-off styles, excessive decoration, and unexplained gradients.
- Support mobile/tablet/desktop. Check real content lengths, Japanese line breaking and text zoom.

## Accessibility / interaction
- Consider WCAG 2.2 AA as a baseline candidate per guideline No.15; confirm the project's applicable requirements. Do not equate automated-check success with full conformance.
- Use WAI-ARIA APG for complex interactive widget keyboard behavior and ARIA patterns, and prefer native HTML semantics.
- Use semantic elements, accessible labels and visible keyboard focus; keep contrast sufficient.
- Use obvious loading, empty, success, validation and error states.
- Prefer reduced motion for users who request it; animation should communicate purpose.
- Do not rely on icon-only controls without accessible names.
- Confirm click/tap targets and mobile navigation.

## CSS implementation
- CSS and utility classes are acceptable. **No utility framework is mandated**.
- Keep reusable tokens and components in the project repo.
- Use project-specific theme variables rather than assuming brand colors from references.

## Review
1. Read the applicable numbered recommendations in [the 30-item guidelines](ui-guidelines.md), especially No.1–16 for the design stage; use No.20–30 for performance, security and release reviews.
2. Compare pages and component states against the shared visual rules.
3. Test keyboard-only and zoomed-in layouts.
4. Test motion reduction and dark mode if supported.
5. Check no third-party icon/image violates the asset license.
6. Note intentional exceptions in the project brief.
