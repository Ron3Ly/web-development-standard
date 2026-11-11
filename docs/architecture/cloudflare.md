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
