# UI Assets and License Checklist

Use assets that fit the product's visual language and accessibility needs. A URL is a **candidate source**, not a blanket license to redistribute content.

| Source | Typical use | Notes |
| --- | --- | --- |
| [Lucide Animated](https://lucide-animated.com/) | Animated action/navigation icons | Check the exact repository/license and version of each asset |
| [ICOOON MONO](https://icooon-mono.com/) | Static monochrome icons | Review site's conditions, especially redistribution / repackaging |
| [Digital Agency illustration / icons](https://www.digital.go.jp/policies/servicedesign/designsystem/Illustration_Icons) | Explanatory icons and illustrations | Confirm the terms for the *specific asset* and attribution rules |
| [freeicon.dev (Motive)](https://freeicon.dev/) | Animated SVG icons | Check item-level provenance and licensing; do not assume everything on the site is CC0 |

## Asset rules
- Prefer static icons by default. Use animation when conveying action, feedback or status.
- Keep visual size, stroke weight, color and alignment consistent across icon sources.
- Provide accessible names when an icon communicates meaning; hide purely decorative icons from assistive technologies.
- Respect `prefers-reduced-motion`; keep essential UI understandable without animation.
- Review source licenses before commercial use, asset editing, redistribution, bundling in public packages or rehosting.
- For freeicon.dev, original Motive assets may use CC0, but **verify the license attached to each chosen asset**.
- For ICOOON MONO, free-use claims do not automatically authorize redistribution as an icon library.
- Keep a per-project log of selected assets, their source URLs, license text/version, capture date, and required attribution.

## Per-asset register template

| Filename / component | Source URL | License checked | Attribution required? | Notes |
| --- | --- | --- | --- | --- |
| example-icon.svg | (exact source URL) | (license + date) | (yes/no) | (usage) |

## References
- https://lucide-animated.com/
- https://icooon-mono.com/
- https://www.digital.go.jp/policies/servicedesign/designsystem/Illustration_Icons
- https://freeicon.dev/
