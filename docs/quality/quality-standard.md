# Webサイト品質・品質保証開発標準（Q-01〜Q-36）

**調査日: 2026-11-17｜標準v1.9 PR提案版**

対象: React / Vinext（Next.js互換）/ RSC / Cloudflare Workers・D1・R2、Googleログイン/匿名認証、GA4/Cloudflare Web Analytics、音楽APIなどを扱うWebサイト・Webアプリ。

この標準は**実装・運用上の品質受入規則**です。ユーザー提供の[6分野30項目](../design/ui-guidelines.md)をそのまま維持し、[SEC-01〜SEC-36](../security/security-standard.md)・[プライバシー標準](../legal/privacy-policy-standard.md)・[ナビゲーション標準](../design/page-navigation.md)と組み合わせます。Qの番号はこのリポジトリ独自で、WCAG等の正式な要求IDではありません。

## 基本原則

1. 品質を**主要ユーザータスクの成功**、実測性能、アクセシビリティ、内容の正確さ、互換性、復旧可能性で評価する。画面の見た目だけやLighthouse点数だけで合格としない。
2. **案件別の品質目標**を定義する。必須機能、対象URL・デバイス、ラボ条件、テスト範囲、責任者、リリース阻止条件を[品質目標雛形](quality-targets.template.md)へ記録する。
3. **事実に基づく判定**：Pass/Fail/Not tested/Not applicableを分ける。CrUX等の実ユーザーデータが無い場合は`Insufficient data`。新規サイトの未計測値を予測で埋めない。
4. **規格と推奨値を混同しない**：WCAG適合目標はプロジェクトが決定。Core Web Vitalsの「良好」しきい値はGoogleの公開基準。Lighthouse 90点や可用性99.9%、API応答p95値は、案件で承認しない限り一律に強制しない。
5. 公開前は[品質レビュー](../workflows/quality-review.md)、[測定手順](measurement-playbook.md)を使い、公開後の[監視](reliability-observability.md)へつなぐ。

## 品質要求一覧

### A. 要件・ユーザー価値

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-01** | 利用者像・目的・主要タスクと成功条件を明示する | ブリーフと代表的利用者シナリオ |
| **Q-02** | ページ/機能/権限ごとの正常・異常系を一覧にする | ルート・機能・権限マトリクス |
| **Q-03** | 主要タスクの完了・失敗・再試行が理解できる画面にする | タスクベースの手動試験 |
| **Q-04** | 品質の数値目標と測定条件・担当・リスク受容者を決める | 品質目標表とオーナー承認 |
| **Q-05** | 利用者のブラウザ/端末/回線/言語に基づき検証範囲を決める | デバイス・ブラウザ検証表 |
| **Q-06** | 空/読込中/エラー/権限不足/保存中/成功の状態を設計する | 画面ステート一覧とUI証拠 |

### B. 機能とデータの正確性

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-07** | 直接URL・更新・別タブ・戻る/進む・共有リンクを機能させる | NAVレビューとE2E |
| **Q-08** | リンク/ボタン/入力フォームの意味と操作結果を一貫させる | 入力/送信/取消のE2E |
| **Q-09** | 検索・絞込・ソート・ページングの結果とURL状態を一致させる | 0件/大量/境界値/戻る操作 |
| **Q-10** | D1/R2の保存・編集・削除・競合・二重送信で整合性を保つ | 隔離データを使う統合/E2E |
| **Q-11** | Googleログイン・匿名ID・アカウント連携・期限切れから復帰できる | ロール別/セッション切れE2EとSEC |
| **Q-12** | Spotify等のAPI/埋め込みの遅延・失敗・欠落時に安全に回復する | スタブを用いた429/タイムアウト/部分成功 |

### C. アクセシビリティ・体験

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-13** | WCAG目標を選択し、代表ページと該当成功基準を対応付ける | WCAG 2.2 AA候補・評価範囲表 |
| **Q-14** | 見出し/ランドマーク/ラベル/代替テキスト/エラーを意味ある形にする | DOM・支援技術/手動確認 |
| **Q-15** | キーボード/焦点/ダイアログ/動的画面遷移を操作可能にする | キーボードと画面リーダー確認 |
| **Q-16** | コントラスト・ズーム/リフロー・日本語長文/折返しを検証する | 200%/必要なリフロー/幅別表示 |
| **Q-17** | 動き・表示更新・タッチ・フィードバックを調整しreduced-motionに対応する | 状態/モーション設定の手動試験 |
| **Q-18** | 自動アクセシビリティ監査に加えて人による評価を行う | WCAG-EMの範囲/サンプル/結果記録 |

### D. 性能・リソース

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-19** | Core Web Vitalsの良好目標 LCP p75 ≤2.5秒、INP p75 ≤200ms、CLS p75 ≤0.1を使う | CrUX/RUM・URL/端末/期間を記録 |
| **Q-20** | 実利用(field)とLighthouse(lab)を別に報告し、データ不足を区別する | CrUX直近28日/PSI収集範囲と条件 |
| **Q-21** | ページ/端末/ネットワーク別のラボ予算・回帰基準を案件で定める | Lighthouse CI、転送量、条件、基準値 |
| **Q-22** | Vinext/RSCの初期表示・Streaming・Hydration・遷移時の応答を確認する | Cloudflare preview/本番相当のブラウザ・trace |
| **Q-23** | 画像/R2/Geist・Noto Sans JP/JSの読込を最適化しCLS等を避ける | Network/性能プロファイル/CLS |
| **Q-24** | 大量データ、D1クエリ、プリフェッチ、外部APIに上限・ページング・劣化対策を設ける | 遅延/大量件数/リクエスト制限の試験 |

### E. 内容・SEO・互換性

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-25** | 文章・画像・音楽アセットの正確さ/権利/更新責任を確認する | 校正・ライセンス台帳・更新履歴 |
| **Q-26** | 公開ページのみ適切なtitle/リンク/canonical/robots/sitemap/HTTP状態を確認する | Google Search Essentials・HTML/HTTP |
| **Q-27** | 日本語/英語のlang、地域別日時/数値・タイムゾーンを確認する | Intlと実地域/混在言語UI |
| **Q-28** | ブラウザ互換性をWeb Platform Baselineと実ユーザー機器から判断する | Chrome/Edge/Firefox/Safari/iOS/Android試験 |
| **Q-29** | 新Web APIやアニメーションの非対応時も主要操作を成立させる | 非対応環境/feature detection試験 |
| **Q-30** | リンク切れ、埋め込み、画像、404/403、クロール不要ページの露出を点検する | リンク/Network/HTTP/ページ別検証 |

### F. テスト・信頼性・リリース

| ID | 共通品質要求 | 検査・証拠例 |
| --- | --- | --- |
| **Q-31** | 主要操作を再現可能な単体/統合/E2Eテストで保証する | Playwright・安定データ・CI結果 |
| **Q-32** | 視覚回帰・手動UI・NAV/SEC/Privacyのレビューを統合する | 差分画像/レビュー履歴 |
| **Q-33** | Workers/D1/R2/外部APIの失敗率・遅延・例外を監視する | Logs/Traces/Metrics/Issues・実アラート |
| **Q-34** | 利用者に意味あるSLI/SLOと測定期間・責任者・例外を案件で定める | SLO定義と計測値（固定値の強制なし） |
| **Q-35** | 障害・データ復元・第三者API停止・デプロイ切戻しをテストする | 障害模擬・バックアップ・復旧記録 |
| **Q-36** | 未実施/失敗/例外を含めたリリース判定と公開後の計測計画を承認する | 品質ゲート・担当・Go/No-Go記録 |

## Core Web Vitalsの扱い

| 指標 | Googleの「良好」しきい値 | 備考 |
| --- | ---: | --- |
| LCP（Largest Contentful Paint） | **2.5秒以下** | 読み込み感覚。フィールドのp75 |
| INP（Interaction to Next Paint） | **200ms以下** | 操作応答。フィールドのp75 |
| CLS（Cumulative Layout Shift） | **0.1以下** | 視覚的安定性。フィールドのp75 |

- **フィールド実測:** PSIのCrUXは通常直近**28日**。URL個別でサンプル不足ならオリジン集計、両方不足なら利用できない場合がある。判定範囲とデバイス群を必ず明記。
- **ラボ診断:** Lighthouseはシミュレーション測定。スコアが90以上でも、実ユーザーp75や実操作時のINPが達成されているとは限らない。INPをTBTから数値変換しない。
- **リリース前/低トラフィック:** フィールドデータの代わりを捏造せず、代表ルートでラボ・実ブラウザの検証、公開後に実ユーザーデータで継続評価。
- **CIの性能予算:** 画面/端末/ネットワークを固定し、スコアや転送量への`warn`/`error`を案件ごとに設定する。

## 品質ゲート

**通常のNo-Go:** 主要タスクが完了できない、重要データが破損する、認証/所有権が重大に破綻する、合意したアクセシビリティ要求下で主要操作が不能、利用継続不能な障害や重大なプライバシー漏えいが残る。SEC/プライバシーの別ゲートも適用する。

**案件ごとに優先度を定める:** Lighthouseスコアの数点差、軽度の描画差、SEO未最適項目、初期のCrUXデータ不足。リスク・影響・期限・オーナーを記録し、Passに偽装しない。

**品質監視:** CWVフィールド、主要タスク成功率、Workersの5xxと処理時間、D1/R2や音楽APIのエラーを公開後に観測する。安全でプライバシーに配慮したログ/トレースを使用。

## 公式出典

- Core Web Vitalsしきい値: https://web.dev/articles/defining-core-web-vitals-thresholds
- PageSpeed Insights（フィールドとラボ・28日）: https://developers.google.com/speed/docs/insights/v5/about
- Lighthouse CI: https://github.com/GoogleChrome/lighthouse-ci
- WCAG 2.2: https://www.w3.org/TR/WCAG22/
- W3C WCAG-EM 2.0 (Group Note, 2026-07-23): https://www.w3.org/TR/WCAG-EM/
- W3C自動検査ツールの限界: https://www.w3.org/WAI/test-evaluate/tools/selecting/
- Playwright Best Practices: https://playwright.dev/docs/best-practices
- Web Platform Baseline: https://web.dev/baseline
- Google Search Essentials: https://developers.google.com/search/docs/essentials
- W3C Internationalization: https://www.w3.org/International/techniques/authoring-html.en
- Cloudflare Workers Observability: https://developers.cloudflare.com/workers/observability/
- Google SRE Error Budget Policy: https://sre.google/workbook/error-budget-policy/
