# Cloudflare / Vinext / RSC セキュリティ実装指針

調査: 2026-11-16。関連: [Security Standard](security-standard.md) / [Threat Model](threat-model.template.md) / [Review](../workflows/security-review.md)。

本書は**実装パターン**であり、実際のApp Router・Vite・Wrangler・Auth SDKのバージョン/設定に合わせて変更する。コード例の適用には各案件での検証が必要。

## 1. Workersの秘密情報・環境

- Cloudflare WorkersではSecrets bindingsを使う。**Wranglerの`vars`は秘密情報の保存先ではない**。`.dev.vars`/`.env`はGit管理対象外とし、production previewの値を分離する。
- `NEXT_PUBLIC_*`、クライアント向けJS、HTML/RSCストリーム、コンソールログ、エラーページ等にAPIシークレットやOAuth refresh tokenが出ないようにする。
- Cloudflare API Token / GitHub Actions Tokenを機能・環境単位の**最小権限**に制限。ローテーション、破棄、担当者退職時のアクセス剥奪を手順化。
- D1/R2バインディング、route、workers.devサブドメイン、公開カスタムドメインの意図しない接続・公開を確認。
- Cloudflareの設定仕様、プランごとの利用機能を確認し、「WAFを設定済み」と事実なしに記述しない。

公式: https://developers.cloudflare.com/workers/configuration/secrets/ 、https://developers.cloudflare.com/workers/wrangler/configuration/

## 2. RSC / Server Actionは公開エンドポイントとみなす

- RSCを用いたサーバー操作は、**コンポーネントが見えないことを認証/認可とみなさない**。Server ActionやRoute Handlerで毎回サーバーセッションを照合し、必要なロール/所有者の権限を検証。
- `'use server'` だけで認可されるわけではない。Client Componentから送られるuserId、tenantId、role、objectKeyは未信頼。
- エラーやストリームレスポンスでSecrets、DBのSQL、内部スタックが露出しないようにする。
- IDベースのデータアクセス/変更は、他人のID指定とゲストからの直接呼び出しをテストする。
- RSCのキャッシュ、認証付きプリフェッチ、共有レスポンスを監査し、ユーザー/テナント間でデータが混ざらないこと。適切な`Cache-Control: private, no-store`等を案件で選ぶ。
- React/React Server DOM、Vinext、`@vitejs/plugin-rsc`の修正済みバージョンをGitHub/React/Vinextのアドバイザリに照らし検査。**Cloudflare WAFがパッチの代わりではない**。

- https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components
- https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components
- https://github.com/cloudflare/vinext/security/advisories

## 3. D1: 値のバインド + ユーザー単位の認可

**疑似例**（`session.userId`はサーバー側で検証されたものを想定）:

```ts
// DB binding, SQL shape and types are project-specific.
// The WHERE clause enforces *one* ownership rule; real roles/tenants need more checks.
const row = await env.DB.prepare(
  "SELECT id, title FROM items WHERE id = ? AND owner_id = ?"
).bind(itemId, session.userId).first();

if (!row) {
  // Handle missing/unauthorized resource without leaking cross-user existence.
}
```

- D1の`prepare().bind()`でSQL**値**を渡す。テーブル名、カラム名、ソート列の**識別子**はバインドできないため、明示的な許可リストから選択。
- ID一致だけでなく、操作に必要なロール・テナント・所有者に対するチェックを維持。管理系の全件更新を安易に許可しない。
- マイグレーションをPRでレビュー。本番のバックアップ・復元/誤削除手順、ログ/個人データの削除方針を確認。
- D1と別のキャッシュ/RSCストリームの間でテナント隔離が壊れないよう検証。

公式: https://developers.cloudflare.com/d1/worker-api/prepared-statements/

## 4. R2: 非公開を前提に設計

- バケット/オブジェクトを**デフォルト非公開**にし、公開バケットと開発用`r2.dev`を点検。非公開ユーザーファイルを誤って公開ドメインへ配信しない。
- ファイル配信時にはサーバー側で本人/権限を検証するか、適切に限定された**短期限・操作限定**のPresigned URLを使用。
- Presigned URLは知っている人が使える**Bearer権限**。URLの漏えい・ログ記録・長寿命化を避け、用途・権限・期限を確認。
- ユーザーが指定したファイル名と保存キーを直接一致させない。拡張子だけで種類を判断しない。サイズ/タイプ/数の上限や不正アップロード、場合により検疫を行う。
- 削除/非公開変更時にCDN/キャッシュから公開状態が残らないか、実際にアクセスをテストする。

公式: https://developers.cloudflare.com/r2/buckets/public-buckets/ 、https://developers.cloudflare.com/r2/api/s3/presigned-urls/

## 5. Google OAuth・匿名ID・外部音楽API

- OAuthでは[ RFC 9700](https://www.rfc-editor.org/rfc/rfc9700.html)とGoogleの公式設定に沿う。Redirect URIの完全一致、取引固有のstate/nonce、PKCE（クライアント種別に従う）、IDトークンのissuer/audience/署名/期限を適切に検証する。
- ログイン前の匿名アカウントとGoogleアカウントの連携は**別IDを誤って統合しない**よう設計。セッション再生成、データの所有権移管、削除/失効をテスト。
- Spotify、Apple Music、YouTube、setlist.fm、MusicBrainz、DeezerのAPIキー/ユーザートークンはサーバー側のみで扱い、必要な権限に絞る。API利用規約やプライバシーも確認。
- サーバー側`fetch`でURLや画像を取得する機能はSSRF対策。可能なら公式APIの固定ホストとサーバー構築URL、DNS・IP/リダイレクト先の検証を行い、時間/サイズを制限する。外部URLをただ`new URL(...)`に通すだけでは不十分。
- Cookieで認証される状態変更には防御済みCSRF方式を採用。`SameSite`単独を普遍的な十分条件としない。クロスオリジン設定は必要最小限。

公式: https://www.rfc-editor.org/rfc/rfc9700.html 、https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html 、https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html

## 6. HTTPヘッダー・CSP・プライベートキャッシュ

| 対策 | 採用ガイド |
| --- | --- |
| HTTPS / HSTS | HTTPSを強制。HSTS `includeSubDomains` / preloadは配下の運用影響を確認した後にのみ検討 |
| CSP | まず`Content-Security-Policy-Report-Only`で計測。React/RSCのinline scriptとNonceやHash、GA4/外部埋め込みの許可を**実HTMLから**検証して強制へ |
| Clickjacking | `frame-ancestors`をCSPで制御。必要に応じ`X-Frame-Options`の互換性も確認 |
| MIME / Referrer | `X-Content-Type-Options: nosniff`、`Referrer-Policy`と適切な`Content-Type`を用途別に設定 |
| CORS | 認証付きAPIは具体的な許可Originのみ。`Access-Control-Allow-Origin: *`とCredentialの組合せは禁止 |
| Cache | 個人情報・セッション固有レスポンスは環境に応じて`private, no-store`等を採用。共有キャッシュ/SSR/RSC/prefetchで異なるユーザーのデータを返さない |
| Rate limit | Cloudflare WAF/rate limitingを使えるプラン/設計に合わせ、アプリ側でもID/トークン/操作費用による制限・異常検知を設ける |

**注:** CSPに`unsafe-inline`を追加してテスト警告を消す、定数nonceを使う、認証をCORSに頼る、キャッシュを切ったと仮定する等は避ける。RSC/Vinextが発するストリーム・スクリプトとCDN挙動を本番相当環境で評価する。

- https://developers.cloudflare.com/workers/examples/security-headers/
- https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html
- https://developers.cloudflare.com/waf/rate-limiting-rules/best-practices/

## 7. 確認すべき主なテスト

1. 未認証・別ユーザー・別ゲストからのD1/R2/RSC操作でIDOR/BOLAが起きない。
2. OAuthコールバックは偽state/不適切なredirectで失敗し、正規フローは成功。
3. Cookie認証の変更操作は適切なCSRF防御が効き、保存は二重送信されない。
4. 不正SQL入力、悪意あるHTML/URL、許可外SSRF先、巨大リクエスト/アップロードを安全に拒否。
5. 匿名アカウントの昇格時に他人のデータへ誤結合しない。
6. R2私有バケット/署名URLが別ユーザーに公開されない。署名URLは期限切れ後に使用できない。
7. CSPが必要なスクリプトを破壊せず、不正な読み込みをブロック。Cookie/TLS/Headers/CORSを検査。
8. Preview/Productionのバインディング、セキュリティスキャン、現行React/RSC/Vinextアドバイザリを確認。

検証記録は [Security Review](../workflows/security-review.md) に残し、未実施のものは`Not tested`と明記する。
