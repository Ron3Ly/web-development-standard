# Standard Technology Stack

## Core

| Category | Default / candidate | Policy |
| --- | --- | --- |
| Frontend | React | Standard UI library |
| Framework | Vinext | Preferred Next.js-compatible framework; verify feature compatibility |
| Rendering | React Server Components | Prefer server components where supported; use client components for browser interaction |
| Navigation / Routing | Vinext App Router + native links / `next/link`, where supported | See [Routing & Navigation](routing-navigation.md) and [Page Navigation UX](../design/page-navigation.md) |
| Build | Vite / Rolldown | Use the versions/toolchain supported by the actual Vinext release |
| Runtime / hosting | Cloudflare Workers | Confirm Workers APIs and deployment settings |
| Relational database | Cloudflare D1 | Use when needed |
| Object storage | Cloudflare R2 | Use when needed |
| Login | Google sign-in / anonymous | Requirements; auth provider/session strategy unselected |
| Analytics | Google Analytics 4 | Optional, consent/privacy review needed |
| Analytics | Cloudflare Web Analytics | Optional |
| Privacy documentation | [Privacy policy standards](../legal/privacy-policy-standard.md) | Prepare data inventory, owner-approved disclosures and release checklist before publishing |
| Security baseline | [SEC-01–SEC-36](../security/security-standard.md) | ASVS 5.0.0 based risk-selected verification, threat model, implementation and release review |
| Quality baseline | [Q-01–Q-36](../quality/quality-standard.md) | User-flow E2E, CWV field/lab, WCAG, browser, content, operational quality |
| Font | Geist Sans + Noto Sans JP (default) / Geist Mono (code) | See [typography standard](../design/typography.md) for alternative presets, loading, licensing and fallback |
| Styles | CSS + utility classes | Utility framework **not specified** |
| UI components (candidate) | [Astryx](https://github.com/meta/astryx) | Optional; React 19+, currently beta. Requires runtime/build/RSC validation |
| Language | JavaScript or TypeScript | **Undecided**: inspect each repo; choose deliberately |
| External integrations | Spotify, Apple Music, YouTube, setlist.fm, MusicBrainz, Deezer | Opt-in per project |

## Decision rules
- Do **not** silently replace Vinext with Next.js, introduce Tailwind/shadcn, Astryx, a database, auth framework or external service without need.
- If Astryx is selected, confirm React 19+, package/peer-dependency compatibility, CSS import order and RSC client boundaries; follow [Astryx guidelines](../design/astryx.md).
- Do not assume full Next.js compatibility. Confirm RSC, routing, middleware/edge compatibility, caching, image handling and deployment against the currently pinned Vinext version. Verify route links, prefetch, scroll restoration, Suspense/loading and transition behavior following [Routing & Navigation](routing-navigation.md).
- Check official docs and pin dependency versions in the project lockfile.
- Configure selected fonts explicitly. For Vinext, inspect the pinned version's `next/font` implementation: it may not match Next.js build-time self-hosting and fallback metrics. Prefer tested WOFF2 assets and CSS `@font-face` when deterministic first-party delivery is required. Validate Cloudflare production requests and font performance. See [typography standard](../design/typography.md).
- Maintain server/client boundaries, avoid leaking secrets or making privileged API calls from the browser.
- For authentication, analytics, D1/R2 or external music APIs, inventory data types, third-party scripts, retention, data subject requests and transborder processing. Cross-check the live behavior with the [privacy standard](../legal/privacy-policy-standard.md) before release.
- Prefer existing package scripts and framework conventions over invented commands.

## Validation checklist
- [ ] Confirm Vinext features used by this project
- [ ] Confirm production build, Cloudflare deployment and env bindings
- [ ] Confirm responsive behavior and keyboard accessibility
- [ ] Confirm chosen Japanese/Latin font stack, font licenses, loading, fallback, glyphs and layout shift
- [ ] Confirm storage permissions and migrations if D1/R2 is used
- [ ] Confirm auth session / anonymous-account migration behavior if auth is used
- [ ] Confirm analytics consent, privacy notices and event data handling if analytics is used

## Security verification

Use [Web Security Standard](../security/security-standard.md) and the [release checklist](../workflows/security-review.md). Before deploying Vinext/RSC, check official React, Vinext and RSC plugin advisories and lockfile-resolved versions, not only package.json. Verify the Cloudflare release in a production-equivalent environment. This standards repository does not host a runnable application or claim to have passed those tests.
## Application quality verification

Before release, apply [Quality Standard](../quality/quality-standard.md) and [Quality Review](../workflows/quality-review.md). Verify actual Vinext/RSC rendering/streaming/hydration on Workers previews, not only the development server. A successful build is not an end-user quality measurement. Use per-project budgets and test matrices.
## 2D game artwork / sound / VFX (optional)

The standard stack does **not** require adding a game engine. For 2D web games only, consult [Game Asset Sources](../game-assets/README.md), [Licensing Rules](../game-assets/licensing.md) and [GAME review](../workflows/game-asset-review.md). Actual game rendering/audio must respect browser user-activation rules, RSC client boundaries, reduced motion, and Cloudflare static/R2 delivery controls. Spotify/Apple Music API access is not a license to redistribute recordings as in-game audio.