---
name: implement-page
description: Implement a website page following the standard React/Vinext/Cloudflare architecture and existing project conventions.
---

# Implement Page
1. Inspect the active project's code, `AGENTS.md`, lockfile, brief, tech-stack docs and `docs/design/ui-guidelines.md` (30-item standard) when available.
2. Check compatibility of each needed Vinext/Cloudflare feature before coding.
3. Reuse components and design tokens. Prefer RSC unless interaction requires client code. If Astryx is chosen by the project, read `docs/design/astryx.md`, use the pinned CLI for component APIs, and validate Vinext/RSC integration before changes.
4. Add only the necessary APIs, bindings, dependencies and permissions.
5. Protect credentials; ensure data authorization and safe error/loading states.
6. Check applicable guideline items: selection/keyboard interactions (1–16), compatibility (17–19), speed and concurrency (20–23), server-side auth and data protection (24–26), and tests/monitoring (27–30). Run applicable lint, typecheck, tests and build from actual package scripts.
7. Report what changed, what was checked and what remains unverified.
