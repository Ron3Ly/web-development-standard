# Webアプリケーション・セキュリティ開発標準（SEC-01〜SEC-36）

**作成・調査日: 2026-11-16｜共通開発標準 v1.8（PR提案）**

対象: 新規Webサイト / 認証付きReact・Vinext / RSC・Vite/Rolldown / Cloudflare Workers・D1・R2 / Googleログイン・匿名認証 / GA4 / 外部音楽API。

> この文書は**プロジェクト内部の開発・検証基準**です。実際のコード、権限、クラウド設定、運用手順を検証せず「安全」「ASVS準拠」と表明しません。セキュリティ対策と、[プライバシーポリシー](../legal/privacy-policy-standard.md)に記載する法定事項は区別します。
>
> 既存の[30項目ガイドライン](../design/ui-guidelines.md)の原文は維持し、主にNo.22、24〜28のセキュリティ領域を具体化した補足規約とします。

## 1. 参照標準の位置づけ

| 基準 | バージョン / 状況（調査日） | この開発標準での使い方 |
| --- | --- | --- |
| [OWASP ASVS](https://asvs.owasp.org/) | **5.0.0 安定版**（2025-05-30） | 技術的セキュリティの要求・検証基準。案件ごとに実際の要求IDと証拠を対応付ける |
| [OWASP Top 10](https://top10.owasp.org/2025/ja/) | **2025版** | リスク分類の確認（アクセス制御、設定、サプライチェーン、暗号、インジェクション、設計、認証、完全性、ログ、例外処理） |
| [OWASP API Security Top 10](https://owasp.org/API-Security/) | **2023版** | APIのBOLA/オブジェクト単位認可、認証、過剰アクセス等 |
| [IPA 安全なウェブサイトの作り方](https://www.ipa.go.jp/security/vuln/websecurity/index.html) | 公式日本語解説 | SQLインジェクション、XSS、CSRF、セッション、不適切なアクセス制御の実装上の対策 |
| [NIST SP 800-218 SSDF](https://csrc.nist.gov/pubs/sp/800/218/final) | **v1.1 正式版**。Rev.1 / v1.2は[ドラフト](https://csrc.nist.gov/pubs/sp/800/218/r1/ipd) | 安全な開発プロセス・脆弱性対応・証跡（PO/PS/PW/RV） |
| [OAuth 2.0 Security BCP](https://www.rfc-editor.org/rfc/rfc9700.html) | **RFC 9700（2025）** | Google等のOAuth認証・リダイレクト・PKCE・CSRF対策 |
| [Cloudflare公式資料](https://developers.cloudflare.com/workers/) | サービス・プラン・バージョン依存 | WorkersのSecrets、D1のbind、R2の非公開・署名URL、WAF / ログ |
| [GitHub公式資料](https://docs.github.com/en/actions/how-tos/secure-your-work) | 機能の利用可否に依存 | 依存関係、CI/CD、Actions権限、認証情報、脆弱性検知 |

**ASVSの適用レベル:** 静的・低リスクの公開ページではL1を初期検討、一般の**ログイン・個人データ・D1/R2を扱うアプリはL2を原則候補**、高価値・高保証が必要なシステムはL3を検討。最終的なレベルはデータの機微性、業務影響と脅威モデルに基づいて案件所有者が選択する。この標準はASVSの全要求を転載・完全実装するものではない。「ASVS L2適合」は実際に該当要求を検証した場合のみ。

**分類:** 「基本」は該当する機能で必須の**社内開発ルール**、「条件付き」は機能や脅威がある場合、「運用」はリリース/保守時のルール。法律・OWASPが以下の一文一文を同じ形で義務付けているという意味ではありません。

## 2. セキュリティ管理基準 36項目

### A. セキュリティ設計と責任（SEC-01〜04）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-01** | 基本 | 資産（URL、API、D1、R2、OAuthトークン、ログ）、ユーザー役割、データ種別、外部サービスを棚卸し | データフロー/ルート/権限一覧 |
| **SEC-02** | 基本 | ユーザーフローと信頼境界を踏まえて脅威モデルを作る。IDOR/BOLA、資格情報流出、CSRF、RSC/サプライチェーン等を評価 | [脅威モデル雛形](threat-model.template.md) |
| **SEC-03** | 基本 | ASVS 5.0.0の目標レベル（L1/L2/L3）と該当要求を選定し、版と確認方法を記録 | 要求ID・対象範囲・チェック記録 |
| **SEC-04** | 運用 | 未対応事項・例外に所有者、理由、期限、代替策を割当て。例外があってもテスト結果を偽装しない | リスク台帳・承認記録 |

### B. 認証・セッション・権限（SEC-05〜12）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-05** | ログイン | Google OAuth/OIDCは実際のクライアント種別に適合した認可コードフロー、PKCE（public clientでは必須、confidential clientでも推奨）、正確なredirect URI照合、取引固有のstate/nonce等のCSRF/リプレイ対策。implicit/password grantを使わない | OAuth設定・コールバック/リプレイ試験 |
| **SEC-06** | セッション | Cookieベース認証ならSecure/HttpOnly/適切なSameSite、スコープ/有効期限、ログイン/権限変更時のセッション更新、ログアウト/失効を実装 | Set-Cookie、期限切れ・失効テスト |
| **SEC-07** | Cookieでの変更 | 状態変更リクエストをCSRFから保護（フレームワークの防御/トークン/検証済みOrigin等、脅威に応じた方法）。SameSite単独を万能な対策と扱わず、GETを変更操作に使わない | 他オリジンPOST拒否・正当な操作継続 |
| **SEC-08** | ゲストあり | 匿名IDを推測不能・権限限定にし、他人のゲストデータへアクセスできない。Googleへの昇格時のアカウント乗っ取り/誤紐付けを防ぐ | 別端末・別匿名ID・昇格試験 |
| **SEC-09** | 管理者あり | 管理者権限を一般ユーザーから分離。必要に応じてMFA/追加認証・監査を設計 | 管理画面の認証/権限/ログ |
| **SEC-10** | データアクセス | Server Action、Route Handler、API、RSCデータ取得それぞれで**サーバー側の認証・認可**を実施。UIの非表示は代替にならない | 未認証・低権限の直接呼出試験 |
| **SEC-11** | ID/テナントあり | D1行、詳細ID、R2オブジェクト、他ユーザー/他テナントに**オブジェクト単位**の権限チェックを設ける。URLのIDを変えても取得・変更できない | 他人IDでの読み/書き/削除の否定試験 |
| **SEC-12** | 基本 | 権限は最小権限、deny-by-default。クライアント入力からrole/userId/tenantIdを信頼しない。重要な変更は監査可能にする | 権限マトリクス・コードレビュー |

### C. 入出力・API保護（SEC-13〜20）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-13** | 入力あり | フィールド/型/桁数/ファイルサイズ/Content-Type/並び順の候補等をサーバー側で制限。予想外の入力は安全に拒否 | 入力検証/413/415/境界値テスト |
| **SEC-14** | D1利用 | SQL値はD1のprepared statement + `bind()` で渡す。テーブル名や列名は**別途許可リスト**から選択。文字列結合で値や識別子を混ぜない | SQL検査・悪意ある入力テスト |
| **SEC-15** | HTML描画 | Reactの通常のエスケープを優先。信頼できないHTML/Markdown/リンクURLの挿入には用途別サニタイズ・URL検証。未検証の`dangerouslySetInnerHTML`を禁止 | XSS試験・不正URL/HTMLの確認 |
| **SEC-16** | サーバー外部fetch | ユーザーの入力URLをWorkersから無制限に取得しない。可能なら固定APIホスト/許可リスト、DNS・IP・リダイレクト先の検査、タイムアウト/サイズ制限でSSRF対策 | 無許可ホスト/内部アドレス/リダイレクト拒否 |
| **SEC-17** | アップロード | ファイル種類・署名/内容・サイズ・回数・所有者をサーバーで検証。公開/私有を区分し、必要なら隔離・検疫・画像変換。アップロード名をそのまま信用しない | 不正形式・超過・他人ファイル試験 |
| **SEC-18** | 基本 | エラーレスポンスに内部構成/SQL/スタック/秘密情報を出さない。正常/失敗時のHTTPコードを使い分け、セキュリティログには必要最小限を記録 | 500/400/403などの応答確認 |
| **SEC-19** | APIあり | 安全なHTTPメソッド、リクエストボディ/処理時間/ページサイズの上限、キャンセル等を定める。公開GETに副作用を入れない | 不正メソッド、巨大入力、タイムアウト試験 |
| **SEC-20** | 公開API/ログイン | ログイン、匿名作成、検索、大量処理、OAuth/API呼出しに乱用制御（identity/IP/route/費用）を設ける。WAFは多層防御であり単独対策としない | 429/制限・正当ユーザー負荷試験 |

### D. ブラウザとHTTP境界（SEC-21〜24）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-21** | 公開サイト | HTTPSを基本とし、HTTPから適切にリダイレクト。HSTSの導入/強制/サブドメイン影響は本番環境で検証 | TLS/リダイレクト/ヘッダー実測 |
| **SEC-22** | JS/CSSあり | CSPをReport-Only→検証→強制へ段階適用。RSCのscript/nonce、GA4等の外部ドメインと調整。XSSの一次防御をCSPに依存しない | ブラウザCSP違反ログと本番HTML |
| **SEC-23** | API/埋込あり | CORSは必要なオリジンに限定。credential付きで`*`を使わない。クリックジャッキング防御、iframe許可元、権限ポリシー等を用途ごとに決定 | Origin/OPTIONS/iframe試験 |
| **SEC-24** | 個人データあり | 個人情報・認証済みRSC/HTML/APIレスポンスのキャッシュ、プリフェッチ、CDNを制御。ユーザー間に応答が混在しないよう確認 | 別ユーザー×戻る/再読み込み/キャッシュ試験 |

### E. Cloudflareと外部連携（SEC-25〜29）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-25** | 秘密情報あり | Workers secretsでトークン管理。`vars`や`NEXT_PUBLIC_*`に秘密を置かず、`.env`/`.dev.vars`をGitへ入れない。漏えい時は失効と再発行 | Wrangler設定、secret scan、ローテーション手順 |
| **SEC-26** | Cloudflare利用 | PreviewとProductionのDB/R2/キーを分離し、Workers bindings・Cloudflare APIトークンの権限を最小化。意図しない公開サービス/管理endpointを無効化 | 環境ごとの設定・権限・公開面 |
| **SEC-27** | D1利用 | 本番DBアクセス/マイグレーションをレビューし、バックアップ・復旧手順と誤削除対策を定義。全ユーザー抽出/更新をできるだけ限定 | migration・復旧試験・権限確認 |
| **SEC-28** | R2利用 | オブジェクトは原則非公開、公開設定/`r2.dev`を点検。署名URLは**有効期限付きBearer権限**として扱い、スコープ/操作/期限/漏えいを管理 | 公開URL・別所有者・署名URL有効期限試験 |
| **SEC-29** | 連携APIあり | Spotify/Apple Music/YouTube等のOAuthトークン・APIキーを最小スコープ/安全保存。再試行、署名検証（Webhook利用時）、削除/失効、API制限、個人データ送信を点検 | 連携解除・失効/レート/不正Webhook試験 |

### F. 脆弱性・サプライチェーン・変更管理（SEC-30〜33）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-30** | React/RSC/Vinext | `react`/`react-dom`/`react-server-dom-*`/`@vitejs/plugin-rsc`/`vinext`等、**実際に解決された全依存**を公式アドバイザリと照合。深刻な既知脆弱性を放置しない | Lockfile、advisory URL、修正コミット |
| **SEC-31** | パッケージ使用 | Lockfileを保存し、依存監査/SCA・Secret scan・SAST/CodeQL等を案件に応じて構成。検出→担当/対応期限→再検査の経路を持つ | Scan結果・対応PR・例外記録 |
| **SEC-32** | GitHub Actions | `GITHUB_TOKEN`は最小権限、サードパーティActionは検証済み固定commit（またはlockfile）、未信頼PRで本番Secretを使わない。OIDCはCloudflare等で実際に対応する方式のみ利用 | CI workflow差分・権限・secret scan |
| **SEC-33** | デプロイあり | レビュー済みPR、検証済みアーティファクト、Preview/Productionの分離、ロールバック方法と本番アクセス権を管理 | Branch/環境保護、署名/レビュー、deploy履歴 |

### G. テスト・監視・インシデント（SEC-34〜36）

| ID | 適用 | 要求事項 | 検証・証跡 |
| --- | --- | --- | --- |
| **SEC-34** | リリース | 主要ロール・攻撃面に対して自動/手動のネガティブテスト、ASVS該当項目、検査不能項目の理由をレビュー。安全で許可された環境のみテスト | [検証表](../workflows/security-review.md)・出力・実行環境 |
| **SEC-35** | 運用 | 不審なログイン/権限拒否/エラー率/大量アクセスを検知・通知。**トークン/個人情報をログに記録しない**。適切な保存期間・アクセス権を設定 | アラート・レダクション・監査証跡 |
| **SEC-36** | 運用 | 脆弱性・秘密漏えい・誤公開・データ事故に備えて隔離、鍵失効、回復、通知要否判定、事後改善と担当者を定める | [インシデント対応手順](incident-response.md)・訓練/記録 |

## 3. リリースブロッカーの考え方

- **リリース不可（通常の初期方針）:** 他人のデータ読書き、重大な認可欠落、既知の悪用可能な重大依存脆弱性、秘密情報の露出、無制限公開R2、CSRF等で重大操作を偽装可能、未修正の重大/高リスク項目。
- **High/Criticalの例外:** 安全上の緊急対応など合理的なケースに限り、**脆弱性の責任者・緩和策・期限・承認者**を記録した上で案件の責任者が判断。証跡なしに「Pass」へ変更しない。
- **Medium/Low:** 重大度、攻撃可能性、データの価値、影響範囲を踏まえ優先順位・担当・期限を管理。
- **Not tested:** 実施した証拠がない。**Passではない**。スキャン実施のみでASVS検証完了とみなさない。
- WAF・レート制限・CSPは**多層防御**。根本的な認可・依存パッチ・入力処理の代わりにはならない。

## 4. React Server Components / Vinext 固有の注意

ReactチームはCVE-2025-55182（RSCの未認証RCE）と、DoS/ソース露出に関わるCVE-2025-55184 / CVE-2025-67779 / CVE-2025-55183 / CVE-2026-23864を公開しています。**初回修正リリースには後から別の修正が必要だったものもあります。** 古い「最初の修正版」一覧だけで安全性を判断しないでください。

- React公式: https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components
- React追加通知: https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components
- Vinextの[Security Advisories](https://github.com/cloudflare/vinext/security/advisories)（2026年10月にキャッシュキーおよびServer Action/RSC関連が公表）
- [GitHub Advisory Database](https://github.com/advisories)

**運用ルール:** プロジェクトのpackage.json + lockfile、RSCビルド/SSR経路を照合し、アドバイザリに記載された「影響バージョン」「修正バージョン」を、その時点の実依存へ当てはめること。Vinext独自の依存/互換レイヤーがあるため、Next.jsのバージョン表をVinextへそのまま適用しない。

## 5. 使い方（GPT Work / Codex）

1. まず[案件ブリーフ](../project/brief.template.md)を完成し、[脅威モデル](threat-model.template.md)をコピーして実データで埋める。
2. セキュリティレベルと適用するSEC項目、リスク所有者を記録する。
3. 認証・D1・R2・OAuth・RSC等は[Cloudflare hardening](cloudflare-hardening.md)に従う。
4. PRごとにセキュリティ影響を確認し、リリース前に[Security Review](../workflows/security-review.md)を実施。
5. 運用の検知・パッチ・事故連絡に[Incident Response](incident-response.md)を使う。
6. 新たなサービス・SDK・OAuth scope・保存先・権限の追加時に再レビューする。

## 6. 主要一次資料

- OWASP ASVS: https://asvs.owasp.org/
- OWASP Top 10:2025: https://top10.owasp.org/2025/ja/
- OWASP API Security: https://owasp.org/API-Security/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
- IPA 安全なウェブサイトの作り方: https://www.ipa.go.jp/security/vuln/websecurity/index.html
- NIST SSDF 1.1 (Final): https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SSDF 1.2 (Draft): https://csrc.nist.gov/pubs/sp/800/218/r1/ipd
- OAuth Security BCP RFC 9700: https://www.rfc-editor.org/rfc/rfc9700.html
- Cloudflare Workers/D1/R2: https://developers.cloudflare.com/workers/ / https://developers.cloudflare.com/d1/ / https://developers.cloudflare.com/r2/
- GitHub Actions Security: https://docs.github.com/en/actions/how-tos/secure-your-work

**本資料のSEC番号はこのリポジトリ独自の管理IDです。ASVSの要求IDを示していません。**
