# 品質レビュー・リリース判定（Q-01〜Q-36）

**2026-11-17**。根拠は[品質標準](../quality/quality-standard.md)、[測定手順](../quality/measurement-playbook.md)および[案件別目標](../quality/quality-targets.template.md)。

**本リポジトリはドキュメントであり、実際のWebアプリの合格実績を示しません。**

## 判定区分

- **Pass:** 対象URL・端末・コミット・日時・実行方法と証拠を持ち、案件の受入条件を満たす。
- **Fail:** 不具合を確認し、影響度・担当・再試験を記録。
- **Not tested:** 環境/実行証拠なし。Passとは別。
- **Not applicable:** 当該機能/対象がなく、説明できる。
- **Insufficient data:** フィールドCWVなど実測母集団が足りない。Lighthouse点数を代替とみなさない。
- **Exception accepted:** Fail等について責任者が期限付きリスクを承認。Passではない。

## 36項目の評価表（案件にコピーして埋める）

| ID | 確認項目 | 検査・証拠例 | 初期状態 |
| --- | --- | --- | --- |
| Q-01 | 利用者像・目的・主要タスクと成功条件を明示する | ブリーフと代表的利用者シナリオ | Not tested |
| Q-02 | ページ/機能/権限ごとの正常・異常系を一覧にする | ルート・機能・権限マトリクス | Not tested |
| Q-03 | 主要タスクの完了・失敗・再試行が理解できる画面にする | タスクベースの手動試験 | Not tested |
| Q-04 | 品質の数値目標と測定条件・担当・リスク受容者を決める | 品質目標表とオーナー承認 | Not tested |
| Q-05 | 利用者のブラウザ/端末/回線/言語に基づき検証範囲を決める | デバイス・ブラウザ検証表 | Not tested |
| Q-06 | 空/読込中/エラー/権限不足/保存中/成功の状態を設計する | 画面ステート一覧とUI証拠 | Not tested |
| Q-07 | 直接URL・更新・別タブ・戻る/進む・共有リンクを機能させる | NAVレビューとE2E | Not tested |
| Q-08 | リンク/ボタン/入力フォームの意味と操作結果を一貫させる | 入力/送信/取消のE2E | Not tested |
| Q-09 | 検索・絞込・ソート・ページングの結果とURL状態を一致させる | 0件/大量/境界値/戻る操作 | Not tested |
| Q-10 | D1/R2の保存・編集・削除・競合・二重送信で整合性を保つ | 隔離データを使う統合/E2E | Not tested |
| Q-11 | Googleログイン・匿名ID・アカウント連携・期限切れから復帰できる | ロール別/セッション切れE2EとSEC | Not tested |
| Q-12 | Spotify等のAPI/埋め込みの遅延・失敗・欠落時に安全に回復する | スタブを用いた429/タイムアウト/部分成功 | Not tested |
| Q-13 | WCAG目標を選択し、代表ページと該当成功基準を対応付ける | WCAG 2.2 AA候補・評価範囲表 | Not tested |
| Q-14 | 見出し/ランドマーク/ラベル/代替テキスト/エラーを意味ある形にする | DOM・支援技術/手動確認 | Not tested |
| Q-15 | キーボード/焦点/ダイアログ/動的画面遷移を操作可能にする | キーボードと画面リーダー確認 | Not tested |
| Q-16 | コントラスト・ズーム/リフロー・日本語長文/折返しを検証する | 200%/必要なリフロー/幅別表示 | Not tested |
| Q-17 | 動き・表示更新・タッチ・フィードバックを調整しreduced-motionに対応する | 状態/モーション設定の手動試験 | Not tested |
| Q-18 | 自動アクセシビリティ監査に加えて人による評価を行う | WCAG-EMの範囲/サンプル/結果記録 | Not tested |
| Q-19 | Core Web Vitalsの良好目標 LCP p75 ≤2.5秒、INP p75 ≤200ms、CLS p75 ≤0.1を使う | CrUX/RUM・URL/端末/期間を記録 | Not tested |
| Q-20 | 実利用(field)とLighthouse(lab)を別に報告し、データ不足を区別する | CrUX直近28日/PSI収集範囲と条件 | Not tested |
| Q-21 | ページ/端末/ネットワーク別のラボ予算・回帰基準を案件で定める | Lighthouse CI、転送量、条件、基準値 | Not tested |
| Q-22 | Vinext/RSCの初期表示・Streaming・Hydration・遷移時の応答を確認する | Cloudflare preview/本番相当のブラウザ・trace | Not tested |
| Q-23 | 画像/R2/Geist・Noto Sans JP/JSの読込を最適化しCLS等を避ける | Network/性能プロファイル/CLS | Not tested |
| Q-24 | 大量データ、D1クエリ、プリフェッチ、外部APIに上限・ページング・劣化対策を設ける | 遅延/大量件数/リクエスト制限の試験 | Not tested |
| Q-25 | 文章・画像・音楽アセットの正確さ/権利/更新責任を確認する | 校正・ライセンス台帳・更新履歴 | Not tested |
| Q-26 | 公開ページのみ適切なtitle/リンク/canonical/robots/sitemap/HTTP状態を確認する | Google Search Essentials・HTML/HTTP | Not tested |
| Q-27 | 日本語/英語のlang、地域別日時/数値・タイムゾーンを確認する | Intlと実地域/混在言語UI | Not tested |
| Q-28 | ブラウザ互換性をWeb Platform Baselineと実ユーザー機器から判断する | Chrome/Edge/Firefox/Safari/iOS/Android試験 | Not tested |
| Q-29 | 新Web APIやアニメーションの非対応時も主要操作を成立させる | 非対応環境/feature detection試験 | Not tested |
| Q-30 | リンク切れ、埋め込み、画像、404/403、クロール不要ページの露出を点検する | リンク/Network/HTTP/ページ別検証 | Not tested |
| Q-31 | 主要操作を再現可能な単体/統合/E2Eテストで保証する | Playwright・安定データ・CI結果 | Not tested |
| Q-32 | 視覚回帰・手動UI・NAV/SEC/Privacyのレビューを統合する | 差分画像/レビュー履歴 | Not tested |
| Q-33 | Workers/D1/R2/外部APIの失敗率・遅延・例外を監視する | Logs/Traces/Metrics/Issues・実アラート | Not tested |
| Q-34 | 利用者に意味あるSLI/SLOと測定期間・責任者・例外を案件で定める | SLO定義と計測値（固定値の強制なし） | Not tested |
| Q-35 | 障害・データ復元・第三者API停止・デプロイ切戻しをテストする | 障害模擬・バックアップ・復旧記録 | Not tested |
| Q-36 | 未実施/失敗/例外を含めたリリース判定と公開後の計測計画を承認する | 品質ゲート・担当・Go/No-Go記録 | Not tested |

実際のプロジェクトでは、各行に**Pass/Fail/Not tested/N/A/Insufficient dataの根拠、スクリーンショット/ログ/テストURL、端末・ブラウザ、担当、確認日**を追記。初期`Not tested`を自動で合格に変更しない。

## PR/リリースのゲート

- [ ] 重要ユーザージャーニー、機能、権限、空/エラー/低速、D1/R2整合性を確認
- [ ] ビルド・静的解析・単体/統合/E2Eの実行結果と未実施が明確
- [ ] 対象ブラウザ/端末、日本語・英語、ズーム/キーボード/スクリーンリーダーを確認
- [ ] Lighthouseは**ラボの条件**と結果を記録。フィールドCWVはp75とURL/期間/端末を記載
- [ ] 公開ページではSEO/リンク/コンテンツの正確さを点検
- [ ] Workersログ・トレース/アラート、外部依存、障害時復旧とロールバックを確認
- [ ] [NAV](navigation-review.md) / [SEC](security-review.md) / [プライバシー](privacy-review.md)の該当ゲートを確認
- [ ] 重大な未修正の不備は原則No-Go。例外は責任者・代替策・期限付きで承認

## 結果記録の雛形

```text
Project/URL/commit: 【要入力】
Quality owner / reviewer: 【要入力】
Browser/device/network: 【要入力】
Q-items executed: 【要入力】
Methods: 【Playwright, manual, Lighthouse, PSI/CrUX, Workers logs etc】
Lab measurements / environment: 【要入力】
Field CWV / p75 / trailing window / page or origin: 【要入力 or Insufficient data】
Failures & untested: 【要入力】
Evidence: 【CI/録画/スクリーンショット/計測結果】
Other gates: 【NAV, SEC, Privacy】
Exception: 【責任者/緩和策/期限 or なし】
Release: 【Go / No-Go / Pending】
```

参照: https://playwright.dev/docs/best-practices / https://developers.google.com/speed/docs/insights/v5/about / https://www.w3.org/TR/WCAG-EM/
