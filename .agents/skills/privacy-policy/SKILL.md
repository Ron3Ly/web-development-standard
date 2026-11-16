---
name: privacy-policy
description: Prepare or review privacy policy drafts and data-handling disclosures for Japan-facing React/Vinext/Cloudflare applications using a verified data inventory. Use when implementing login, analytics, cookies, third-party APIs, data deletion, or editing public privacy notices.
---

# Privacy Policy — Evidence-Based Draft and Review

1. Read `AGENTS.md`, `docs/legal/privacy-policy-standard.md`, `docs/legal/data-inventory.template.md`, `docs/workflows/privacy-review.md`, and the active project's brief.
2. Inspect actual implementation: auth/OAuth scopes and Google Cloud settings; session/anonymous ID; Cloudflare Workers/D1/R2/logs; analytics tags (GA4 and Cloudflare Web Analytics); third-party music APIs and embedded media; deletion flows and customer support process.
3. Build a project-specific **data flow inventory**. For each data class, capture collection source, purpose, exact recipients, legal classification (if confirmed), storage/processing, retention and deletion path, and evidence. Mark missing facts **未確認**; never fabricate company name, address, recipients or promises.
4. Check applicable law **as of the intended publication date**: Japan APPI and 2026 amendments' actual commencement, Telecommunications Business Act external-transmission applicability, overseas transfer rules, relevant regional/sector laws, and Google OAuth/GA4/provider contracts.
5. Only after verifying facts, draft from `docs/legal/privacy-policy.template.md`. Remove unused provider examples and unused sections, replace placeholders, and map paragraphs to supporting inventory evidence.
6. Include accessible navigation to the publicly readable policy page, with appropriate links from the website footer and login/registration/collection moments. Confirm requirements of OAuth privacy URL and consent screen where relevant.
7. Run PR-01–PR-20 from `docs/workflows/privacy-review.md` against the actual site/settings. Distinguish Pass, Fail, Not tested and Not applicable; provide concrete evidence for each.
8. **Never declare legal compliance or publish unverified wording.** Require approval by the site operator and legal review where appropriate; report open questions, blockers, suggested text changes, and any needed UI/engineering changes.

This is a workflow for generating defensible drafts and implementation checks, not professional legal advice.
