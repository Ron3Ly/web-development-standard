# UI Design, UX & Accessibility Guidelines

> Reviewed links: 2026-11-11. This catalog is part of the shared **Web Development Standard** used by GPT Work and Codex. It defines which source to consult for each design decision; it does not require installing every design system's code.

## 1. Reference catalog (user-specified)

| No. | Source | Official URL | Main application | Status in this standard |
| --- | --- | --- | --- | --- |
| 1 | Nielsen Norman Group (NN/g) | https://www.nngroup.com/ | UX research, usability heuristics, information architecture, user journeys, feedback | Advisory research/guidance, not a normative technical standard |
| 2 | Microsoft Learn — Windows Design | https://learn.microsoft.com/en-us/windows/apps/design/ | Windows platform conventions, Fluent interaction principles, navigation, desktop input patterns | Context-specific reference; not mandatory for ordinary websites |
| 3 | W3C WAI-ARIA Authoring Practices Guide (APG) | https://www.w3.org/WAI/ARIA/apg/ | Accessible widgets, roles, states, properties, focus and keyboard interaction | Practical implementation patterns; not a replacement for WCAG requirements |
| 4 | Digital Agency Design System (デジタル庁デザインシステム) | https://design.digital.go.jp/ | Japanese-language UI, typography, forms, components, accessibility, understandable information structure | Principal design reference, especially for Japan-facing services; URL may redirect to /dads/ |
| 5 | IBM Carbon Design System | https://carbondesignsystem.com/ | Enterprise UI, complex tables, forms, components, tokens, layout patterns | Optional pattern/reference system; no default dependency on Carbon React |
| 6 | W3C Web Content Accessibility Guidelines (WCAG) 2.2 | https://www.w3.org/TR/WCAG22/ | Testable accessibility success criteria for web content | **Default target: Level AA**, subject to applicable product and legal requirements |

## 2. How to combine them

- **Accessibility quality target:** Design and review against applicable **WCAG 2.2 Level AA** success criteria unless the project has an explicitly documented different target or legal requirement. A successful automated scan is not proof of WCAG conformance.
- **Widget behavior:** Use **native semantic HTML first**. For custom widgets (dialog, tab list, combobox, menu, tree, etc.), consult **WAI-ARIA APG** patterns and test actual keyboard, focus and screen-reader behavior. APG examples are informative implementation guidance, not a guarantee of conformance.
- **Usability and UX:** Use **NN/g** research and heuristics to evaluate clarity, learnability, error prevention, feedback and user flow; tailor findings to evidence from the current audience.
- **Japanese UX and visual consistency:** Use the **Digital Agency Design System** as the first stop for Japanese-language forms, information hierarchy, input patterns, typography and interface clarity. Its government-oriented branding need not be copied.
- **Desktop / Windows contexts:** Consult **Microsoft Windows Design** when a site aims to reproduce desktop-app behavior, supports Windows-centric keyboard/mouse patterns, or is a Windows application. Do not force Windows-only design conventions onto cross-platform responsive websites.
- **Complex enterprise interfaces:** Consult **IBM Carbon** for data-dense layouts, complex forms, tables, filtering and enterprise workflows. Use component patterns as a reference; installing Carbon's runtime library is an independent project decision.
- **Component implementation:** [Astryx](astryx.md) is a separate optional React component system; it does not supersede these guidelines. Use documented APIs, and evaluate compatibility with React 19, Vinext, RSC and Cloudflare.

## 3. Priorities when advice conflicts

1. Applicable laws, security, privacy, product/user requirements and contractual accessibility obligations.
2. Testable accessibility success criteria and usable keyboard/screen-reader behavior appropriate to the agreed WCAG target.
3. The current audience's needs, real user research and platform/device constraints.
4. Shared visual tokens, components and the project's own product/brand conventions.
5. Advisory heuristics and optional component-pattern examples.

Do **not** treat NN/g, Windows Design, Digital Agency, Carbon or APG as identical kinds of standards. Don't mix competing visual systems indiscriminately. Adopt the interaction principle, not branding, proprietary copy or unrelated dependencies.

## 4. Design & review checklist

### Before implementation
- [ ] Define audience, language, target devices, common tasks and interaction complexity.
- [ ] Set the accessibility target (default WCAG 2.2 AA) and any binding legal/contractual requirements.
- [ ] Select relevant guidance sources and cite the exact page/pattern used.
- [ ] Identify required components, native HTML alternatives, keyboard interactions and error states.
- [ ] Choose one coherent visual-token system; document any intentional departures.

### During implementation
- [ ] Semantic headings, landmarks, forms, labels, descriptions and status messages.
- [ ] Keyboard access, visible focus, logical tab order and focus management.
- [ ] Appropriate ARIA roles, states and properties **only where needed**.
- [ ] Sufficient contrast, non-color cues, text resizing and responsive reflow.
- [ ] Accessible authentication and form error handling.
- [ ] Reduced-motion preferences and understandable interactions without animation.

### Before delivery
- [ ] Run available automated accessibility tests and manual keyboard checks.
- [ ] Test relevant APG widget behaviors and consider assistive-technology testing.
- [ ] Review mobile and desktop layouts with realistic Japanese copy.
- [ ] Inspect relevant WCAG 2.2 Level AA criteria and record pass/fail/not-tested evidence.
- [ ] Document usability concerns and the specific NN/g, Digital Agency, Windows or Carbon guidance applied.
- [ ] Explicitly record unverified criteria. Never claim complete conformance based on a checklist or a linter alone.

## 5. Source usage and updates

- Revisit the official source for current guidance when starting a substantial design or audit; these sites evolve.
- Link to sources instead of copying large portions of copyrighted documentation or proprietary design assets.
- If adding a third-party component library, review its license, versions, dependencies, styling collisions and runtime compatibility before adoption.
- Log design decisions in the project's brief or design record using: **decision / requirement / source URL / reason / validation / date**.
