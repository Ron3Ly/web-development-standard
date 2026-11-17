---
name: quality-review
description: Define and verify quality for React/Vinext/Cloudflare web sites and apps, including user journeys, WCAG, Core Web Vitals, Playwright, SEO, browser compatibility, and SLOs.
---

# Evidence-Based Web Quality Review

1. Read `AGENTS.md`, `docs/quality/quality-standard.md`, `docs/quality/quality-targets.template.md`, `docs/quality/measurement-playbook.md`, `docs/quality/reliability-observability.md`, `docs/workflows/quality-review.md`, and the active project's brief. Keep NAV, SEC and privacy criteria separate.
2. Inventory critical user flows, public/protected routes, Google/guest login, D1/R2 and external music integrations, target locales, browsers and devices.
3. Choose applicable Q-01–Q-36 checks and define project-owned targets/budgets. Field CWV p75 good thresholds are LCP ≤2.5s, INP ≤200ms, CLS ≤0.1. Lighthouse lab scores do not substitute for real-user data. Do not invent a 99.9% SLO or require 90+ on every site.
4. Execute available build, integration/E2E (Playwright), manual accessibility/browser and performance checks on a real app with authorization, stable test data and evidence. Do not claim a docs repository has an app or that unrun tests passed.
5. Review Workers preview behaviour, RSC streaming/hydration, direct navigation/back, guest→Google login, D1/R2 correctness, third-party failure, RUM/lab comparison and available Cloudflare metrics.
6. Record Pass/Fail/Not tested/Not applicable and Insufficient data for absent field metrics. Report environment, URL, date, source, commit and responsible party for findings. Do not claim WCAG conformance based only on automated tools.
7. Create clear, minimal PR corrections and report release blockers, owners, mitigation, retest and post-launch monitoring. Do not merge without explicit instructions.
