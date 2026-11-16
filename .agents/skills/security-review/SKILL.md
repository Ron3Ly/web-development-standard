---
name: security-review
description: Threat model, implement and review secure web applications using OWASP ASVS 5.0.0, OWASP Top 10:2025, NIST SSDF and Cloudflare/Vinext/RSC-specific controls. Use for security architecture, auth, D1/R2 APIs, code reviews, or pre-release security audits.
---

# Security Review (evidence-based)

1. Read `AGENTS.md`, `docs/security/security-standard.md`, `docs/security/cloudflare-hardening.md`, `docs/security/threat-model.template.md`, `docs/workflows/security-review.md`, and the active project's brief. If personal data is involved, read `docs/legal/privacy-policy-standard.md` and the project's data inventory.
2. Determine the project scope, asset list, auth/anonymous roles, tenant/data ownership, RSC/Server Actions/API routes, Cloudflare D1/R2/Workers bindings and provider integrations. Do not infer production configuration from README alone.
3. Select and justify an ASVS 5.0.0 level: L1 for low-risk starting scope; normally consider L2 for authenticated/personal-data systems, L3 for heightened assurance. Choose the **actual ASVS requirements**, not just this repository's SEC IDs.
4. Build/update the threat model and risk register. Explicitly check IDOR/BOLA, OAuth replay/redirect, CSRF, XSS, SQLi, SSRF, storage exposure, secrets, RSC dependencies/caches, CI/CD and supply-chain risks.
5. Inspect package-lock/pnpm/yarn resolved versions, React RSC and Vinext advisories, auth provider behaviors, migrations, Cloudflare config and public/private asset routes. Never claim a vulnerability is fixed unless the installed version is confirmed.
6. Run **authorized, non-destructive** verification for applicable SEC-01–SEC-36 items, ideally against staging. Do not test credentials, denial-of-service or destructive attacks against unapproved production systems.
7. Provide Pass/Fail/Not tested/Not applicable, evidence and remediation for each. Distinguish unresolved critical/high issues from low risk and obtain named risk-owner review for any exceptions.
8. Suggest changes in a PR with clear risks, test plan and rollback guidance. Do not silently weaken CSP, CSRF, access control, dependencies or secret handling merely to pass automated tests.
9. When finished, summarize changed files, checks executed, limitations, residual risks and approval/release decisions. **Do not claim certification or complete compliance without independent verification.**

Reference: `docs/security/incident-response.md` for live incidents.
