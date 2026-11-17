# セキュリティレビューとリリース判定（SEC-01〜SEC-36）

作成日: 2026-11-16。関連: [要求一覧](../security/security-standard.md) / [脅威モデル](../security/threat-model.template.md) / [Cloudflare対策](../security/cloudflare-hardening.md) / [事故対応](../security/incident-response.md)。

**目的:** セキュリティ要求を検証可能な証跡に結び付け、リリース/PR時のリスク受容を明確にする。**この文書の存在のみでアプリのセキュリティは検証されません。**

## 1. ステータスと必要証跡

- **Pass:** 本番相当または適切な隔離環境でチェックを行い、テスト手順/実行者/日付/出力/コード/構成を記録。
- **Fail:** 不備が確認された。課題/影響/優先度/修正担当/再テストを記録。
- **Not tested:** 実行できていない。理由と予定を記載。**Passに置き換えない。**
- **Not applicable:** 当該機能・脅威がないことを実コード・設定から説明。
- **Exception accepted:** Fail/Not testedのリスクを責任者が期限・緩和策付きで受容した状態。**Passではない。**

最終承認者は対象の実プロジェクトの責任者。AIは検証結果を整理するが、法的・技術的適合を認定しない。

## 2. SEC項目と最小検査

| ID | チェック / 推奨テスト | 証跡（記入例） | 判定 |
| --- | --- | --- | --- |
| SEC-01 | 全API/データ/外部接続/役割の棚卸し | ルート表・依存一覧 | Not tested |
| SEC-02 | 攻撃面と信頼境界、重大脅威の文書化 | 脅威モデル、レビュー記録 | Not tested |
| SEC-03 | ASVSの版/要求/レベルと範囲を決定 | 選定メモ・実要求ID | Not tested |
| SEC-04 | リスク例外の期限と承認者 | リスク受容記録 | Not tested |
| SEC-05 | Google OAuth PKCE/nonce/state/redirect検証 | 設定・偽コールバック試験 | Not tested |
| SEC-06 | Session Cookieの属性/失効/更新 | Response headerと操作テスト | Not tested |
| SEC-07 | Cookie認証のCSRF/GET非変更試験 | 外部Origin拒否・既定防御検証 | Not tested |
| SEC-08 | 匿名ID/昇格の他人データ混在防止 | 別ユーザー/別端末テスト | Not tested |
| SEC-09 | 管理者認証/権限/MFA設計（該当時） | ロール・管理UIの証跡 | Not tested |
| SEC-10 | Server Actions/Route Handlers/RSCの直叩き認可 | 未認証/低権限APIテスト | Not tested |
| SEC-11 | 他ユーザー/他テナントD1・R2 BOLA | 別IDで読み/更新/削除拒否 | Not tested |
| SEC-12 | 最小権限とクライアントrole偽装拒否 | マトリクス・異常入力テスト | Not tested |
| SEC-13 | 型/長さ/範囲/ボディ制限 | 境界値・413/415等 | Not tested |
| SEC-14 | D1の`bind`と識別子allowlist | SQL検査・インジェクション試験 | Not tested |
| SEC-15 | HTML/Markdown/URLでXSS防止 | 入力・表示・DOM検査 | Not tested |
| SEC-16 | SSRF防止、ホスト/DNS/リダイレクト検証 | 許可外URL拒否 | Not tested |
| SEC-17 | R2アップロードの所有者/種類/サイズ制限 | 偽MIME・巨大ファイル等 | Not tested |
| SEC-18 | 例外・詳細情報・Secretsの非露出 | 4xx/5xxとログ確認 | Not tested |
| SEC-19 | 安全なHTTPメソッドと時間/量の上限 | メソッド/サイズ/タイムアウト試験 | Not tested |
| SEC-20 | 匿名/ログイン/重いAPIのレート/費用制限 | 429/観測/正常利用 | Not tested |
| SEC-21 | TLS/HTTPリダイレクト/HSTSの運用 | curl/ブラウザ/network | Not tested |
| SEC-22 | CSP Report-Only→強制とXSS多層防御 | 応答ヘッダーと違反ログ | Not tested |
| SEC-23 | CORS/iframe/frame-ancestors等 | 不正Origin/埋込テスト | Not tested |
| SEC-24 | private/no-storeと跨ユーザーCache隔離 | 2ユーザーのURL/戻る/再読込試験 | Not tested |
| SEC-25 | Workers secrets、漏えいscanとローテーション | config/scan/権限記録 | Not tested |
| SEC-26 | Preview/Prod・Bindings・権限分離 | 環境/Cloudflare設定差分 | Not tested |
| SEC-27 | D1 migration/バックアップ/復元 | migration/リストア証跡 | Not tested |
| SEC-28 | R2非公開/署名URL期限/所有者 | Bucket設定/URL試験 | Not tested |
| SEC-29 | 外部API最小スコープ/トークン/SDK | OAuth/API/ログ/契約設定 | Not tested |
| SEC-30 | React/RSC/Vinext/プラグイン修正版を確認 | resolved lockfile + advisory | Not tested |
| SEC-31 | SCA/SAST/secret scanと修正追跡 | 脆弱性通知・scan結果 | Not tested |
| SEC-32 | CI workflow/Action固定/最小権限 | workflow/Actions監査 | Not tested |
| SEC-33 | Protected deploy/レビュー/rollback | GitHub/Cloudflare構成、履歴 | Not tested |
| SEC-34 | 手動/自動セキュリティテスト | QAレポート/結果/範囲 | Not tested |
| SEC-35 | 重要イベント監視とPII/Secretのログ除外 | alert/retention/ログ試験 | Not tested |
| SEC-36 | 事故対応手順/担当/連絡/訓練 | runbook/模擬訓練記録 | Not tested |

> 各プロジェクトでコピーして**実施後の状態に更新**する。表の初期`Not tested`はこの標準リポジトリがWebアプリを試験したことを意味しません。対象外となった行も、理由を記録して`Not applicable`に変更。

## 3. 代表的なネガティブ・セキュリティテスト

| テスト | 期待結果 | 方法 |
| --- | --- | --- |
| 未ログインで保護URL/Server Actionを直接呼び出し | セッション無しでデータを返さない | 許可済み隔離環境 |
| 利用者AのIDを利用者Bへ置換（Read/Update/Delete） | 403/404等、安全な不許可応答 | 2アカウント/2ゲスト |
| OAuthコールバックの偽state/不一致redirect | 認証に失敗し、安全なURLへ戻る | 生成したテスト用コールバック |
| Cookieを使った変更APIへの別Origin POST | 変更は拒否、正当な操作は成功 | CSRF検査 |
| スクリプト入り入力を表示 | HTML/DOMで実行されない | 保存/反映のテスト |
| 改変したSQL入力/並び順 | 他データを読まず安全に拒否 | Parameterized query確認 |
| 外部URL取得を許可外ホスト/redirectへ | 禁止ネットワーク・ホストへリクエストしない | SSRF安全な模擬先 |
| 未承認R2/private object/keyへアクセス | 認可失敗・署名期限切れ後アクセス不可 | 2ユーザー/署名テスト |
| RSC再利用・キャッシュを2アカウントで切替 | レスポンスに他人の情報が混ざらない | Preview Workers＋Network |
| GA4/外部SDKを含む本番CSP | 正当な読み込みが維持され、不許可Scriptはブロック | CSPレポート・ブラウザ |
| 失効済みsecret/API token | 使用不能、事故手順で再発行可能 | 管理済みテストキー |

**テストは許可されたステージング/ローカル環境のみで行う。** 実データを使わず、過負荷・大量リクエスト・外部サイトへの攻撃をしない。自動監査を実行しただけで手動の認可/プライバシー検証を完了したことにしない。

## 4. GitHub PR / デプロイゲート

**PR段階**
- [ ] セキュリティ影響・権限/データフローの差分を記載
- [ ] 依存/Secrets/CIの変更は人間がレビュー
- [ ] CIの既存テストと検証済みのSCA/SAST/Secret scan（利用可能な範囲）を実行
- [ ] React/RSC/Vinextのセキュリティアドバイザリとlockfile解決版を確認
- [ ] 新設/変更した権限/API/Storageについてネガティブテスト
- [ ] Fail/Not testedを記録し、リスクオーナー/修正予定を明示

**リリース段階**
- [ ] 重大/高リスクの未修正事項について公開不可か、期限付き承認済み例外かを判定
- [ ] Preview/Prodの異なるSecrets、D1/R2、カスタムドメインを確認
- [ ] 秘密情報・個人情報の漏えいがないことをレスポンス・RSC・ログで確認
- [ ] 監視・バックアップ/復元・切戻し・インシデント連絡先を準備
- [ ] 実際の公開環境のセキュリティヘッダー/CORS/Cache/401/403/404を確認

## 5. 実施報告テンプレート

```text
Project: 【案件名】
Commit/Release: 【SHA】
Target: ASVS 5.0.0 L【】（採用理由・適用範囲）
Environment: 【local / preview / staging / production-equivalent】
Executed: 【SEC-xx / テスト名 / 方法 / 取得日時】
Result: 【Pass / Fail / Not tested / Not applicable】
Evidence: 【実行ログ / ブラウザ録画 / コード・設定URL（秘密は除く）】
Outstanding: 【未解消/確認不可のリスク】
Risk owner: 【担当】
Exception: 【承認者、理由、緩和策、期限】 or なし
Release decision: 【Go / No-Go / Pending】
```

## 6. 出典と実行ツール

- [OWASP ASVS 5.0.0](https://asvs.owasp.org/) — 適用範囲を選択
- [OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/) — セキュリティ検証手法
- [OWASP Top 10:2025](https://top10.owasp.org/2025/ja/) — リスク分類
- [IPA 安全なウェブサイトの作り方](https://www.ipa.go.jp/security/vuln/websecurity/index.html) — 国内向け解説
- [GitHub Code Scanning](https://docs.github.com/en/code-security/concepts/code-scanning/code-scanning) / [Dependabot](https://docs.github.com/en/code-security/how-tos/secure-your-supply-chain/secure-your-dependencies/configure-dependabot-alerts) — 警告
- [Cloudflare Workers](https://developers.cloudflare.com/workers/) / [D1](https://developers.cloudflare.com/d1/) / [R2](https://developers.cloudflare.com/r2/) — 実際の配備環境
