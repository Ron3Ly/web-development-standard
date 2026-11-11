# Standard Technology Stack

## Core

| Category | Default / candidate | Policy |
| --- | --- | --- |
| Frontend | React | Standard UI library |
| Framework | Vinext | Preferred Next.js-compatible framework; verify feature compatibility |
| Rendering | React Server Components | Prefer server components where supported; use client components for browser interaction |
| Build | Vite / Rolldown | Use the versions/toolchain supported by the actual Vinext release |
| Runtime / hosting | Cloudflare Workers | Confirm Workers APIs and deployment settings |
| Relational database | Cloudflare D1 | Use when needed |
| Object storage | Cloudflare R2 | Use when needed |
| Login | Google sign-in / anonymous | Requirements; auth provider/session strategy unselected |
| Analytics | Google Analytics 4 | Optional, consent/privacy review needed |
| Analytics | Cloudflare Web Analytics | Optional |
| Font | Geist | Add Japanese fallback when relevant |
| Styles | CSS + utility classes | Utility framework **not specified** |
| Language | JavaScript or TypeScript | **Undecided**: inspect each repo; choose deliberately |
| External integrations | Spotify, Apple Music, YouTube, setlist.fm, MusicBrainz, Deezer | Opt-in per project |

## Decision rules
- Do **not** silently replace Vinext with Next.js, introduce Tailwind/shadcn, add a database, auth framework or external service without need.
- Do not assume full Next.js compatibility. Confirm RSC, routing, middleware/edge compatibility, caching, image handling and deployment against the currently pinned Vinext version.
- Check official docs and pin dependency versions in the project lockfile.
- Maintain server/client boundaries, avoid leaking secrets or making privileged API calls from the browser.
- Prefer existing package scripts and framework conventions over invented commands.

## Validation checklist
- [ ] Confirm Vinext features used by this project
- [ ] Confirm production build, Cloudflare deployment and env bindings
- [ ] Confirm responsive behavior and keyboard accessibility
- [ ] Confirm storage permissions and migrations if D1/R2 is used
- [ ] Confirm auth session / anonymous-account migration behavior if auth is used
- [ ] Confirm analytics consent, privacy notices and event data handling if analytics is used
