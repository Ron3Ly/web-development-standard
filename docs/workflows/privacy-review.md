# プライバシーポリシー公開前・変更時レビュー（PR-01〜PR-20）

作成日: 2026-11-16 / 対象: 日本向けWebサービスおよび適用する他国の法令。
関連: [記述標準](../legal/privacy-policy-standard.md) / [公開用テンプレート](../legal/privacy-policy.template.md) / [データ台帳](../legal/data-inventory.template.md)。

**レビューの結果は Pass / Fail / Not tested / Not applicable とする。**
法令上の義務、契約上の要件、推奨される透明性の区分を明記する。AI自身が法令準拠を保証したり、実装確認なしにPassとしたりしない。

## 実施プロセス

1. **情報収集・棚卸し**: コード/Cloudflare/GTM/Google OAuth/実サービス設定/契約を確認。
2. **適用法の判定**: 日本APPIの現行施行版、電気通信事業法の外部送信規律、適用される海外法・個別分野法、Google等のサービス利用規約。
3. **台帳・ドラフト**: 情報の種類ごとに目的・処理・送信先・期間を文案に対応させる。
4. **実装/運用レビュー**: 実際の収集や保存・削除の仕組みと公開文言が一致するか確認。
5. **運営者/必要に応じ法務の承認**を経て公開。改定日と版を保管。
6. **定期/変更時に再点検**: 新SDK、タグ、OAuth scope、データ保持、D1/R2保存、第三者委託、法改正、対象国などの変更を検知したら再審査。

## PR-01〜PR-20 チェックマトリクス

| ID | 確認項目 | 合格条件（実務上の受入基準） | 対象 |
| --- | --- | --- | --- |
| PR-01 | 運営者情報 | 実在する正式名称・連絡先・法令上の該当公表事項（住所/代表者等）が確認されている | 全案件 |
| PR-02 | データ取扱台帳 | 実際の入力項目・Google/匿名ID・Cookie・D1/R2・ログ・解析・API/埋込を網羅 | 全案件 |
| PR-03 | 取得情報と目的 | 種別ごとに取得源・目的・用途の根拠が明確でコード/設定と一致 | 収集あり |
| PR-04 | APPI 取得時説明/公表 | 利用目的の特定、通知・公表の実施方法を現行法に照らして確認 | 対象事業者 |
| PR-05 | 保有個人データの公表等 | 法32条/施行令の氏名/名称・住所・代表者・目的・請求方法・安全管理・苦情窓口等を確認 | 対象事業者 |
| PR-06 | 委託/第三者提供/共同利用 | 実際の相手先、法的分類、同意/公表等の要件を区別し裏付ける | 該当時 |
| PR-07 | 外国への提供・取扱い | 国/相手先/適用例外/情報提供/同意・契約を個別に評価 | 該当時 |
| PR-08 | Cookie・外部送信台帳 | ブラウザNetwork、タグ/GTM、SDK、埋込と送信項目・送信先が一致 | 外部送信あり |
| PR-09 | 電気通信事業法 | 対象役務・対象者・送信情報・例外を判定し、必要な通知/公表等を実装 | 該当時 |
| PR-10 | Googleログイン | スコープとGoogleユーザーデータ取扱いの記述、OAuth審査/公開同一ドメイン/リンク要件を確認 | Google OAuthあり |
| PR-11 | GA4 | GA4の利用・Cookie/識別子・収集/処理の説明、同意/停止の適法性、PII送信防止 | GA4あり |
| PR-12 | Cloudflare Web Analytics | 実ビーコン/データフローを確認。GA4と区別し、根拠なくCookieなし等と言わない | CFWAあり |
| PR-13 | D1/R2・ログ | 保存内容、アクセス権、保持期間、バックアップ、削除、所在・委託先が確認済み | 該当時 |
| PR-14 | 外部音楽API・埋め込み | 採用サービスのみ記述し、OAuth・アカウント連携・埋込送信を点検 | 該当時 |
| PR-15 | 権利行使と問い合わせ | 開示/訂正/利用停止等・本人確認・退会・手数料・苦情対応窓口が実在し運用可能 | 該当時 |
| PR-16 | 安全管理措置 | 実際の措置と文言が一致。安全上重要な内部詳細は開示しない | 全案件 |
| PR-17 | 年齢・地域別追加法 | 子供向け/GDPR等の適用を確認、該当する情報を記載。2026 APPI改正の施行状況を再確認 | 該当時 |
| PR-18 | 公開ページUX | `/privacy` 等に直接アクセス可能、ログイン不要、フッター・登録/認証等から到達、モバイル/アクセシビリティ/印刷可読性 | 全案件 |
| PR-19 | 変更履歴・表示内容 | 制定/更新日が正しく、無関係なサービス・プレースホルダー・架空情報・未実施の約束がない | 全案件 |
| PR-20 | 立証・承認 | 実装/設定/Network/契約/法令の証拠、最終レビュー者と必要な承認・改訂手順が記録済み | 全案件 |

「全案件」はこの**開発標準でのデフォルト**です。法律上の一律の法定義務と同一視しない。

## 検証すべき実装例

- ブラウザで直接 `/privacy` を開き、未ログイン/モバイル/キーボードでも閲覧できること。
- 初回訪問時・同意前/後・ログイン前/後にNetworkやStorageを検査し、実際のGA4/Cloudflare/埋め込みの外部送信を一覧化。
- OAuth同意画面に登録したURLと公開URLが一致し、必要な場合同一ドメインの要件を満たすこと。
- Googleユーザーデータ/セッション/匿名ID/D1/R2の保有と退会・開示/削除の運用が政策文書と一致。
- プライバシー関連のユーザー操作（設定変更・拒否・連携解除）が、必要とされる場合に実際のタグ/処理を制御すること。
- 保存期間、ログ、サポート記録、バックアップの例外を含む正しい文面になっていること。
- DOMとHTTPアクセス、公開ページ、メタデータの連続性を確認（ポリシーページが認証必須・404にならない）。

## 変更を検知したら再審査する条件

- Googleログインのスコープ追加・第三者OAuth連携開始・音楽API/埋込追加
- GA4、Cloudflare Web Analytics、広告タグ、外部フォント、外部SDK、エラー監視サービスの導入・変更
- D1/R2データ項目や保管地域、ユーザー削除/復旧仕様、保持期間の変更
- 利用目的・広告/分析用途・外国提供・委託先・共同利用の変更
- 対象年齢/提供地域の変更、法令/Google等利用規約の改正

## PRレビュー記録ひな型

| ID | Status | 根拠（画面・Network・設定・コード・公式資料） | 担当者 | 対応 |
| --- | --- | --- | --- | --- |
| PR-01 | Not tested | 【要入力】 | 【要入力】 | 【要入力】 |
| PR-10 | Not applicable | Google OAuth未採用ならその根拠 | 【要入力】 | - |
| PR-20 | Not tested | 運営者承認が未取得 | 【要入力】 | 公開前に承認 |

レビュー者：【要入力】 / 実施日：【YYYY-MM-DD】 / 公開承認：【未承認】 / 対象リリース：【要入力】

## 公式一次資料と実務資料

- 個人情報保護委員会: https://www.ppc.go.jp/personalinfo/legal/guidelines_tsusoku/
- 2026改正: https://www.ppc.go.jp/personalinfo/legal/r8kaiseihogohou/
- 通則QA: https://www.ppc.go.jp/personalinfo/faq/APPI_QA/
- 電気通信事業法: https://laws.e-gov.go.jp/law/359AC0000000086
- Google API Services User Data Policy: https://developers.google.com/terms/api-services-user-data-policy
- Google OAuth verification: https://developers.google.com/identity/protocols/oauth2/production-readiness/brand-verification
- Google Analytics Terms: https://marketingplatform.google.com/about/analytics/terms/us/
- Cloudflare Web Analytics: https://developers.cloudflare.com/web-analytics/about/
- GDPR（対象となる場合）: https://commission.europa.eu/law/law-topic/data-protection/information-individuals_en
