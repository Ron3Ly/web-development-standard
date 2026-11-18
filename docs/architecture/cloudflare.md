# Cloudflare Architecture & Security

## Default infrastructure
- **Cloudflare Workers**: deployment and server execution (subject to Vinext compatibility).
- **Cloudflare D1**: relational data if the application needs it.
- **Cloudflare R2**: object storage if the application needs it.
- **Cloudflare Web Analytics**: lightweight web analytics when enabled.

## Operational rules
1. Keep runtime secrets in appropriate Cloudflare secret bindings, never checked into Git or delivered to client bundles.
2. Define environment variable names and binding names in the **project repository**, not as invented universal bindings.
3. Use D1 migrations with reviewable SQL; avoid direct mutation of production databases from development.
4. Scope database queries to authorized users; anonymous users still require robust authorization and abuse prevention.
5. Keep R2 objects private by default; expose using checked authorization or appropriately scoped short-lived access when required.
6. Define caching and retention policies, especially for third-party API data and any personally identifiable information.
7. Keep local, preview and production environment behavior distinct.
8. Validate the actual deployed Workers runtime, not only Vite's local development server.
9. Inventory and document which D1 tables, R2 buckets/objects, application logs, backups, access logs and analytics flows may contain personal data or identifiers. Define retention and deletion behavior, processors, applicable region/cross-border handling, and verify what is factually true for the chosen Cloudflare configuration. See [privacy data inventory](../legal/data-inventory.template.md).
10. Do not treat Cloudflare Web Analytics' privacy-first claims as proof that every external-transmission or local disclosure obligation is inapplicable. Check the actual beacon, other tags, and the service scope.

## Required project decisions
- Worker deployment method and config
- D1 database bindings, schema and migration procedure
- R2 buckets, object naming, ACL/authorization and lifecycle
- Secret names and rotation plan
- Monitoring and rollback
- Region/privacy/retention requirements

## References
- https://developers.cloudflare.com/workers/
- https://developers.cloudflare.com/d1/
- https://developers.cloudflare.com/r2/
- https://developers.cloudflare.com/web-analytics/

## Security hardening

For operational measures, read [Cloudflare Hardening](../security/cloudflare-hardening.md) and [SEC-25–SEC-29](../security/security-standard.md). Workers secrets are not ordinary Wrangler `vars`; do not commit `.dev.vars`/`.env`. Check D1 query bindings and migration permissions, R2 public-domain/`r2.dev` exposure and short-lived presigned bearer URLs. Review WAF and rate limits alongside application-level identity-aware controls; Cloudflare WAF is not a substitute for server-side authorization or patching vulnerable RSC dependencies.
## Site quality / service level

Read [Reliability and Observability](../quality/reliability-observability.md) and [Quality Standard Q-31–Q-36](../quality/quality-standard.md). Monitor relevant Workers exceptions, 5xx, latency, D1/R2 binding errors and upstream service failures; define a project-specific SLI/SLO, alert owner, rollback and recovery. Do not equate GA4/Cloudflare Web Analytics visitor counts with runtime reliability monitoring.
## Game assets (2D / optional)

Before hosting 2D game sprites, icons, sound effects, music or VFX on Cloudflare R2, read [Game Asset Licensing](../game-assets/licensing.md) and [Web Implementation](../game-assets/web-implementation.md). R2 public URLs can expose the original audio/image files; licensed permission to **incorporate** an item into a commercial game is not automatically permission to redistribute the raw asset as a stand-alone file, source pack or template. Verify origin/CORS/cache/size, object access controls and copyright before upload.