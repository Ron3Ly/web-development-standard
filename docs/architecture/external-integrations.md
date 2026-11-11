# External Music Integrations

All items are **optional**. Introduce only when a product feature requires them.

| Service | Typical use | Primary consideration |
| --- | --- | --- |
| Spotify | Music catalog, artist/track metadata, account integration | OAuth scopes, API quotas, policy and content restrictions |
| Apple Music | Catalog and library integration | Developer tokens, music-user authorization where necessary |
| YouTube | Video, channels and content metadata | API quotas, embeds and platform policy |
| setlist.fm | Performance/setlist data | API access, terms and attribution |
| MusicBrainz | Music metadata and identifiers | Rate limits, licensing, attribution and stable identifiers |
| Deezer | Music catalog/metadata | Availability, access requirements and usage terms |

## Shared implementation rules
1. Prefer official APIs; confirm current availability, pricing, permissions and terms at implementation time.
2. Store API credentials in server-side secrets. Never embed restricted tokens in browser code.
3. Use a dedicated server-side integration module per provider, with typed input/output shapes if using TypeScript.
4. Implement pagination, timeouts, retries with backoff, rate-limit handling, cache expiration and error fallback.
5. Treat artists/recordings/releases/performances as different entities; do not assume IDs match across services.
6. Define source attribution and obey restrictions on storing artwork, lyrics, audio, videos and metadata.
7. Do not download/rehost protected media or bypass provider playback and embed restrictions.
8. Minimize personal data, request the least scopes and implement token revocation when user accounts are linked.

## Questions for each project
- Which APIs are used, for what exact feature?
- Does the feature require logged-in users or only public data?
- Which rights, attribution, cache duration and data retention apply?
- How do we handle missing matches and cross-service metadata conflicts?

## Official references
- https://developer.spotify.com/documentation/web-api
- https://developer.apple.com/documentation/applemusicapi
- https://developers.google.com/youtube/v3
- https://api.setlist.fm/docs/1.0/index.html
- https://musicbrainz.org/doc/MusicBrainz_API
- https://developers.deezer.com/
