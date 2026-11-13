# AI Agent Instructions — Web Development Standard

This repository is a **shared standards reference**, not an executable application. Respond in Japanese unless the project requests otherwise.

## Required reading
Before planning, designing, or implementing a website:
1. Read `docs/architecture/tech-stack.md`.
2. Read `docs/architecture/cloudflare.md` and `docs/architecture/authentication.md` when relevant.
3. Read `docs/design/ui-guidelines.md`, `docs/design/design-system.md`, **`docs/design/typography.md`**, `docs/design/ui-references.md`, and `docs/design/ui-assets.md`. Read `docs/design/astryx.md` when evaluating or using UI components.
4. Read `docs/architecture/external-integrations.md` if using music APIs.
5. Read the active project's brief. Use `docs/project/brief.template.md` if none exists.

## Technical defaults
- React, Vinext, RSC where supported, Vite/Rolldown, Cloudflare Workers.
- D1 for relational storage, R2 for objects, **only when required**.
- Google login and anonymous/guest authentication are possible requirements, **not a preselected auth library**.
- GA4 and Cloudflare Web Analytics when specified and privacy requirements permit.
- **Typography default for new Japan-facing websites:** use Geist Sans for Latin/UI numerals and Noto Sans JP for Japanese text, with OS Japanese sans-serif fallbacks; use Geist Mono for code/monospaced data only. Actually configure the fonts in the project, not merely mention them. Read `docs/design/typography.md` and use `examples/typography.css` as the starting token/stack reference. Existing projects and project briefs may override the default.
- **Alternate typography presets by context:** Noto Sans JP or BIZ UDPGothic for Japanese readability-focused content; Inter for Latin-heavy products; IBM Plex Sans JP for dense enterprise UI; Noto Serif JP for editorial titles; Zen Maru Gothic for friendly-brand titles. Do not load all candidates.
- CSS with utility classes. **Do not assume Tailwind or shadcn/ui is mandatory**.
- [Astryx](https://github.com/meta/astryx) is an **optional React 19+ UI component/design-system candidate**, not a mandatory dependency. It is in beta; verify Vinext/RSC/Vite/Cloudflare compatibility before adoption.
- JavaScript or TypeScript: the original preference is unresolved; inspect existing code and confirm for greenfield work.
- Music integrations (Spotify, Apple Music, YouTube, setlist.fm, MusicBrainz, Deezer) are **opt-in**.

## Design defaults
- Use `docs/design/ui-guidelines.md` as the **primary 30-item application-development guideline** across six areas: UX/usability (1–7), UI/information design (8–14), accessibility/environment (15–19), performance/reliability (20–23), security/data protection (24–26), and QA/operations/improvement (27–30). Apply context-relevant recommendations; the source explicitly distinguishes standards, design guidance and best practices.
- Consider WCAG 2.2 Level AA as the accessibility target candidate identified in guideline No.15; confirm the actual project requirement. Use APG patterns for keyboard/ARIA interaction guidance (No.16), preferring native semantic HTML. Do not claim conformance without verification.
- Follow the source document's prioritization: first detail No.1–14 and No.15–16 for interactive web applications; also address applicable performance, security and QA practices before delivery. Use the Digital Agency Design System for relevant Japanese-language UI patterns.
- Consider Astryx as a component implementation resource and design-system reference, not a replacement for the Digital Agency design principles. If selected, consult the pinned project's Astryx CLI/component documentation; do not invent component APIs.
- Prefer existing components and tokens, consistent responsive layout, meaningful interaction and restrained motion.
- Limit loaded font families and weights; use Japanese-specific glyph/subset optimization, WOFF2, sensible `font-display`, system fallback and font license notices. On Vinext, do not assume Next.js `next/font` self-hosting/subsetting; validate the pinned version and Cloudflare production build.
- Approved candidate asset sources: Lucide Animated, ICOOON MONO, Digital Agency illustration/icon assets, freeicon.dev.
- Check **specific asset licenses** before copying or redistributing. Respect reduced-motion preferences.
- References are inspiration, **not permission to copy branded UI**.

## Execution process
1. Inspect project requirements, repository structure, lockfile and the current dependency versions.
2. Verify Vinext compatibility and Cloudflare runtime/bindings for the required features.
3. Document assumptions and deviations before installing new frameworks/libraries.
4. Implement minimal, reusable, accessible code; protect secrets server-side.
5. Where available, run the project's lint, type check, tests and production build.
6. Review the applicable 30-item guideline checks, including responsive behavior, selection and table workflows, WCAG/APG interactions, Core Web Vitals, server-side authorization, privacy, E2E tests and monitoring as relevant. Validate actual font loading, Japanese glyph coverage, 200% zoom, fallback and CLS; distinguish recommendations from mandatory requirements and report unverified checks.
7. Report changed files, validation, known limitations and deviations.

## Rules for other repositories
A `GitHub URL` to this repository **does not automatically install or activate these instructions**. Copy the relevant standard into each target repo, or explicitly make these files accessible to GPT Work / Codex and direct the agent to read them. Codex's automatic AGENTS.md discovery follows the **working repository's hierarchy**, not unrelated repositories.

## Conflicts
Security, applicable law, user requirements, accessibility and actual runtime compatibility take precedence. Record exceptions in the active project's brief. Do not treat proposals here as verified implementations.
