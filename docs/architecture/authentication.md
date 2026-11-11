# Authentication Standard

## Product requirements
The common authentication patterns are:
- **Google sign-in**
- **Anonymous / guest access**

These are **capabilities**, not a mandated auth provider or SDK.

## Design checklist
- Choose the specific auth library/provider compatible with Vinext and Cloudflare Workers.
- Define whether anonymous users are authenticated with persistent server-side identities or are simply unauthenticated guests.
- Document session storage, expiration, cookie attributes, CSRF protection and OAuth callback URLs.
- Use least-privilege OAuth scopes; do not store third-party secrets in D1 plaintext.
- Define how an anonymous account is linked/upgraded to Google sign-in without losing data.
- Authorize every protected operation on the server, including access to D1 rows and R2 objects.
- Prevent abuse with rate limits, anti-automation controls and reasonable anonymous-user quotas.
- Specify account deletion, data retention and privacy handling.

## Open decisions
- Provider / session library: **not yet selected**.
- Guest identity and persistence model: **not yet selected**.
- Requirements for email, profile photos, linked music accounts and consent: **project-specific**.
