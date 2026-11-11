# Astryx — Optional React UI Component System

- Source repository: https://github.com/meta/astryx
- Docs: https://astryx.atmeta.com
- Component Storybook: https://astryx.atmeta.com/storybook/
- Package guide: https://github.com/meta/astryx/blob/main/packages/core/README.md
- CLI guide: https://github.com/meta/astryx/blob/main/packages/cli/README.md
- License: MIT (repository license); preserve notices when distributing copied source or substantial portions.
- Status at review (2026-11-11): **Beta**. Recheck the release and changelog before production adoption.

## Position in this standard

**Digital Agency Design System (DADS) remains the principal UI/UX and accessibility design reference.**
**Astryx is an optional implementation library and component-pattern reference** for React projects.

Astryx is not an icon asset repository and is **not** automatically installed in every project. Continue to use the separate asset sources documented in [ui-assets.md](ui-assets.md).

## What Astryx offers

- 150+ React UI components with documented APIs and TypeScript support.
- Design tokens, CSS-variable themes, dark mode and several ready-made themes.
- Templates for common page layouts and component patterns.
- An agent-oriented CLI to discover component APIs, templates, tokens and theming.
- Components authored internally with StyleX; consumers use prebuilt CSS and may customize via CSS and className without setting up a StyleX build pipeline.

## Prerequisites and adoption checks

- Requires **React 19+**. Confirm the active Vinext, React, Vite/Rolldown and Cloudflare Workers versions together; **Vinext compatibility is not established merely because Astryx documents Next.js and Vite setups**.
- Check React Server Component boundaries. Interactive components/providers may need `'use client'`; inspect official component usage and verify SSR/hydration.
- Decide whether the UI library adds value relative to the project's existing component system. Avoid introducing competing global resets/themes without a plan.
- Pin package versions and test the deployed Cloudflare runtime, visual appearance, routing, keyboard accessibility and performance.
- Follow the project's existing language (JavaScript or TypeScript) and CSS approach; Astryx does **not** require switching to Tailwind or to StyleX authoring.

## Optional project installation (only if adopted)

The official README documents these packages and peer dependency:

```bash
pnpm add @astryxdesign/core @astryxdesign/theme-neutral @stylexjs/stylex
pnpm add -D @astryxdesign/cli
```

Follow the package's **current** instructions for theme setup, imports and providers. In the documented basic setup, CSS import order is significant:

```css
@import '@astryxdesign/core/reset.css';
@import '@astryxdesign/core/astryx.css';
@import '@astryxdesign/theme-neutral/theme.css';
```

This snippet is an example for an Astryx-enabled project, **not an instruction to change the CSS of this standards repository**. Existing CSS reset, Tailwind layers, theme and Vinext-specific routing integration need separate validation.

## Workflow for GPT Work / Codex

If the project has approved Astryx:
1. Read the [component package guide](https://github.com/meta/astryx/blob/main/packages/core/README.md), and inspect the pinned CLI version.
2. Discover components rather than inventing APIs:
   ```bash
   pnpm exec astryx component --list
   pnpm exec astryx component Button
   pnpm exec astryx template --list
   ```
3. The official `@astryxdesign/cli init` command can add a component index to **the active project's** `AGENTS.md` / `CLAUDE.md`. Only run it in the correct project repo **after reviewing its effect**, and review the generated diff so existing agent instructions are not lost.
4. Reuse documented components, component states, templates and tokens where they fit the page's requirements.
5. Verify desktop/mobile design, keyboard behavior, bundle impact, RSC hydration and deployment compatibility. Record any deviations.

For one-off CLI use, prefer the **scoped package** (`npx @astryxdesign/cli ...`). The unscoped name `npx astryx` may resolve to an unrelated package.

## When not to adopt

- Existing stable design system already meets the requirements.
- React version is older than 19 or the app cannot safely upgrade.
- Vinext/RSC/Cloudflare compatibility is unverified or fails tests.
- A beta dependency is not acceptable for the project's support/security policy.

In those cases, use Astryx documentation only as a *design-pattern reference* if useful, without adding runtime dependencies.
