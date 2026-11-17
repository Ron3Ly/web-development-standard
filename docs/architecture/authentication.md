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
- For login return paths, accept only validated same-origin local paths or server-side approved destination IDs; never redirect to unvalidated user-supplied URLs. Keep the originally requested destination after successful auth where authorized; avoid redirect loops and exposing credentials in query parameters. See [Routing & Navigation](routing-navigation.md).
- Use least-privilege OAuth scopes; do not store third-party secrets in D1 plaintext.
- Define how an anonymous account is linked/upgraded to Google sign-in without losing data.
- Authorize every protected operation on the server, including access to D1 rows and R2 objects.
- Prevent abuse with rate limits, anti-automation controls and reasonable anonymous-user quotas.
- Specify account deletion, data retention and privacy handling.
- Document actual Google OAuth scopes (even if only sign-in), fields stored, account-linking behavior, token handling and revocation/deletion. Google API Services requires clear privacy disclosures; if an app undergoes OAuth verification, confirm a public privacy policy page on the app's domain and accurate consent-screen links. See [privacy-policy standard](../legal/privacy-policy-standard.md).
- Confirm the guest/anonymous identifier lifecycle and which data, if any, can be linked to a later Google account. Do not claim that anonymous access necessarily means the user cannot be identified.

## Open decisions
- Provider / session library: **not yet selected**.
- Guest identity and persistence model: **not yet selected**.
- Requirements for email, profile photos, linked music accounts and consent: **project-specific**.

## Security-specific baseline

Apply [SEC-05–SEC-12](../security/security-standard.md) and [Cloudflare hardening](../security/cloudflare-hardening.md). For Google OAuth/OpenID Connect, confirm exact redirect URI matching, transaction-specific state/nonce and PKCE as applicable to client type under [RFC 9700](https://www.rfc-editor.org/rfc/rfc9700.html); never use implicit/password grants. Revalidate sessions on account linking, control cookie flags and protect cookie-authenticated writes from CSRF. Require server-side object/tenant authorization even after login; guest IDs are not permission to read other guests' data. Check actual auth provider implementation before asserting protection.