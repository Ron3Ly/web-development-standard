# Cloudflare / Vinext サービスの信頼性・可観測性

**2026-11-17** / [品質規約](quality-standard.md) / [SEC](../security/security-standard.md) / [Cloudflare設計](../architecture/cloudflare.md)。

## SLI/SLOを案件別に設計

| 対象 | SLI候補 | 定義・注意 |
| --- | --- | --- |
| 主要ユーザータスク | ログイン/検索/保存の成功率 | 利用者にとっての成功を優先 |
| Workers | 5xx率、処理時間、例外 | 意図した4xxと障害を区別 |
| D1/R2 | クエリ/オブジェクト操作のエラーと遅延 | 実保存の失敗とレスポンスを関連づける |
| 音楽API | 429/タイムアウト/部分取得失敗 | 外部障害を隠さず回復導線を設ける |
| RUM | LCP/INP/CLS分布 | 代表デバイス・URL/期間と母集団を記録 |

SLO（99.9%等）を全Webサイトで固定しない。期間・リクエスト母集団・除外条件・運用責任者・アラート方式とセットで定める。[Google SREのエラーバジェット](https://sre.google/workbook/error-budget-policy/)は考え方の参考資料。特定の停止基準は案件の意思決定。

## Cloudflareで観測する内容

公式[Workers Observability](https://developers.cloudflare.com/workers/observability/)にはLogs、Traces、Metrics/Analytics、Issuesがある。**利用可能な機能・プラン・設定と実際の収集を確認**。トレースには個人情報/OAuthトークン/認証Cookieを無制限に記録しない。

## 故障と復旧の基準

- 低速/タイムアウト/ネットワーク断/外部API障害時にローディング、失敗、再試行、部分成功を表示。
- 副作用のある更新の無条件再試行を避け、二重保存や競合を防ぐ。
- D1/R2の復旧・バックアップ・予備の表示方法と、Preview/Productionの隔離を確認。
- データを保有するサービスではRTO/RPOを案件別に定義し、バックアップを復元する手順を試す。
- 切戻し対象にスキーマ・機能互換性・過去の重大脆弱性が残らないこと。
- 公開後に不具合、CWV悪化、利用者タスク失敗、サポート問い合わせを追跡し改善の責任者と期限を設ける。

**この標準を保存するだけでは、Cloudflareの監視・アラート・バックアップは設定されません。**
