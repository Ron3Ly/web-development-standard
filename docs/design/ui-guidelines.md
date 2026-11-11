# Webアプリケーション開発ガイドライン一覧（全30項目）

業務用Webアプリケーション（データ一覧、検索、編集、複数選択、一括処理など）を主な対象として、**6分野・30項目**の設計方針と参考ガイドラインをまとめたものです。

## 参照資料の分類

- **標準・規格**：WCAGなど、準拠基準として利用できるもの
- **設計ガイドライン**：Microsoft、IBM Carbon、GOV.UKなど、具体的なUIの実装方針を示すもの
- **ベストプラクティス**：NN/g、MDN、OWASPなど、設計判断や技術選定の根拠になるもの

> **注意：**「推奨される設計方針」は各参照資料を踏まえた実務向けの要約です。すべてが規格上の必須要件というわけではありません。採用時には対象ユーザー、対応環境、製品要件に応じて調整してください。

## 1. UX・ユーザビリティ設計（7項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 1 | 操作性 | 複数選択はCtrl・Shift操作だけに依存せず、チェックボックスなども用意する。ドラッグ操作には代替手段を設ける | [Microsoft：選択モード](https://learn.microsoft.com/ja-jp/windows/apps/develop/ui/controls/selection-modes)、[WCAG：Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements) |
| 2 | 直感性 | ユーザーに馴染みのある用語・アイコン・操作モデルを採用する。機能を推測させない | [NN/g：10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) |
| 3 | 一貫性 | 同じ機能は画面をまたいでも同じ名称・操作・配置・動作に統一する | [NN/g：Consistency and Standards](https://www.nngroup.com/articles/ten-usability-heuristics/) |
| 4 | 操作効率 | 熟練ユーザーにはショートカットキーや一括操作を提供し、初心者も通常のUIで操作できるようにする | [NN/g：Flexibility and Efficiency of Use](https://www.nngroup.com/articles/ten-usability-heuristics/) |
| 5 | フィードバック | 保存中、処理中、成功、失敗などを適切なタイミングで明示する | [NN/g：Visibility of System Status](https://www.nngroup.com/articles/visibility-system-status/) |
| 6 | エラー防止・復旧 | 入力制約、確認、Undo機能を導入し、エラー発生時には原因と修正方法を説明する | [GOV.UK：Error message](https://design-system.service.gov.uk/components/error-message/) |
| 7 | 学習容易性 | 主要機能は説明なしでも発見できる構成とし、複雑な操作には文脈に沿ったヘルプを設ける | [NN/g：Help and Documentation](https://www.nngroup.com/articles/ten-usability-heuristics/) |

**設計上のポイント：** 操作方法を標準化する際には、画面の見た目だけでなく、選択状態、フォーカス、キーボード操作、操作取消時の挙動まで定義すると、画面間の一貫性を高められます。

## 2. UI・画面・情報設計（7項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 8 | 情報アーキテクチャ | ユーザーの業務や目的に基づいて情報を分類する。メニュー階層を複雑にしすぎない | [GOV.UK：Navigate a service](https://design-system.service.gov.uk/patterns/navigate-a-service/) |
| 9 | ナビゲーション | 現在位置と移動先を明確にする。階層が深い場合はパンくずリストを検討する | [GOV.UK：Breadcrumbs](https://design-system.service.gov.uk/components/breadcrumbs/) |
| 10 | レイアウト | 情報の重要度に応じて配置を決め、余白と文字サイズの体系を統一する | [Carbon：Spacing](https://www.carbondesignsystem.com/building-blocks/foundations/spacing/overview) |
| 11 | データ表示 | 表形式データにはテーブルを使用し、階層データにはツリー、内容重視の情報にはカードなどを使い分ける | [Carbon：Data table](https://www.carbondesignsystem.com/building-blocks/core/components/data-table/guidelines) |
| 12 | 検索・絞り込み | 検索、フィルター、ソートを用途に応じて提供し、選択中の条件・並び順が分かるようにする | [Carbon：Data table guidelines](https://www.carbondesignsystem.com/building-blocks/core/components/data-table/guidelines) |
| 13 | 入力フォーム | 入力ラベルを明示し、入力条件を事前に説明する。エラーでは具体的な訂正方法を示す | [デジタル庁：インプットテキスト](https://design.digital.go.jp/dads/components/input-text/usage/) |
| 14 | デザインシステム | 色・文字・余白・アイコン・UI部品を共通ルールとして定義し、再利用可能にする | [Carbon：Themes・Design Tokens](https://www.carbondesignsystem.com/building-blocks/foundations/themes/overview) |

**設計上のポイント：** データテーブルの行選択、一括操作、ソート、ページネーションなどは、コンポーネント単位で共通仕様として定義すると再利用しやすくなります。

## 3. アクセシビリティ・利用環境（5項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 15 | アクセシビリティ | 文字と背景のコントラスト、フォーカス表示、スクリーンリーダーへの対応を行う。WCAG 2.2 AAを基準候補とする | [W3C：WCAG 2.2 Understanding](https://www.w3.org/WAI/WCAG22/understanding/) |
| 16 | キーボード操作 | Tab、Enter、Esc、矢印キーなどの操作を定義し、選択状態とキーボードフォーカスを区別する | [W3C：WAI-ARIA Patterns](https://www.w3.org/WAI/ARIA/apg/patterns/) |
| 17 | レスポンシブ対応 | 画面幅に応じてレイアウトを変更する。拡大表示時にも必要な情報・操作を維持する | [MDN：レスポンシブウェブデザイン](https://developer.mozilla.org/ja/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) |
| 18 | ブラウザ互換性 | 対応ブラウザ・バージョンを定義し、利用するWeb APIやCSSの互換性を確認する | [web.dev：Baseline](https://web.dev/baseline) |
| 19 | 多言語・地域対応 | 言語、日付、時刻、数値、通貨の表示を地域に応じて変更し、タイムゾーンも考慮する | [MDN：国際化](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Internationalization) |

**具体的な基準例：**

- 通常テキストのコントラスト比：原則 **4.5:1以上**（WCAG AA。例外あり）
- キーボード操作時：フォーカス位置を視覚的に識別できる
- ドラッグによる操作：原則としてドラッグなしでも同じ機能を実行できる
- 画面幅・表示倍率の変化：情報や機能を失わないようにする（複雑な表などには例外あり）

## 4. 性能・信頼性（4項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 20 | パフォーマンス | ページ読み込み、操作への応答、表示の安定性を測定する。LCP・INP・CLSなどを指標にする | [Google：Core Web Vitals](https://web.dev/articles/vitals?hl=ja) |
| 21 | 大量データ対応 | 大量データは一度にすべて描画せず、ページネーション、遅延読み込み、仮想スクロールなどを検討する | [MDN：Lazy loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Lazy_loading)、[Carbon：Data table](https://www.carbondesignsystem.com/building-blocks/core/components/data-table/guidelines) |
| 22 | 信頼性・障害復旧 | 通信障害時の再試行、タイムアウト、データ保護、バックアップと復旧手順を設計する | [Microsoft：再試行パターン](https://learn.microsoft.com/ja-jp/azure/architecture/patterns/retry)、[AWS：Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/) |
| 23 | 同時編集・整合性 | 更新競合を検出し、他ユーザーの変更を意図せず上書きしないようにする。楽観的ロックなどを検討する | [MDN：HTTP条件付きリクエスト](https://developer.mozilla.org/ja/docs/Web/HTTP/Guides/Conditional_requests) |

**Core Web Vitalsの推奨目標値（実利用データの75パーセンタイル）：**

| 指標 | 対象 | 良好とされる目安 |
|---|---|---|
| LCP | 読み込み性能 | 2.5秒以下 |
| INP | 操作への応答性 | 200ミリ秒以下 |
| CLS | レイアウトの安定性 | 0.1以下 |

※ 業務検索APIやバッチ処理などの応答時間目標は、Core Web Vitalsとは別に定義する必要があります。

## 5. セキュリティ・データ保護（3項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 24 | 認証・認可 | MFAを検討し、最小権限の原則を適用する。サーバー側で操作やデータへのアクセス権限を確認する | [OWASP：Authentication](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)、[OWASP：Authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) |
| 25 | セキュリティ対策 | XSS、インジェクション、アクセス制御の不備などを防ぐ。セキュリティ要件を開発・テストに組み込む | [OWASP Top 10:2025 日本語版](https://top10.owasp.org/2025/ja/)、[OWASP ASVS](https://asvs.owasp.org/) |
| 26 | データ保護 | 個人データの利用目的、アクセス制御、保存期間、削除・廃棄、暗号化、安全管理措置を定義する | [個人情報保護委員会：ガイドライン（通則編）](https://www.ppc.go.jp/personalinfo/legal/guidelines_tsusoku/) |

**設計上のポイント：** 認証・認可の要件はUI表示の制御だけでは満たせません。API・サーバー側でも権限を検証する必要があります。

## 6. 品質管理・運用・改善（4項目）

| No. | 項目 | 推奨される設計方針 | 参照ガイドライン |
|---|---|---|---|
| 27 | テスト・品質保証 | ユーザーが実行する主要操作をE2Eテストで検証する。テストの独立性を保ち、アクセシビリティは人手でも確認する | [Playwright：Best Practices](https://playwright.dev/docs/best-practices)、[W3C：Accessibility Evaluation](https://www.w3.org/WAI/test-evaluate/) |
| 28 | 監視・障害対応 | エラー率、応答時間、可用性などを監視する。ユーザーへの影響に基づいてアラートを設計する | [Google SRE：Monitoring](https://sre.google/workbook/monitoring/) |
| 29 | 保守性・拡張性 | コンポーネントを再利用し、機能間の依存を減らす。APIの契約やバージョン管理を明確にする | [Microsoft：アプリケーションの設計原則](https://learn.microsoft.com/ja-jp/azure/architecture/guide/design-principles/) |
| 30 | 利用分析・改善 | タスク完了率、所要時間、操作エラー、ユーザー満足度などを測定し、改善に反映する | [MeasuringU：Task-Based UX Metrics](https://measuringu.com/task-based-metrics/) |

## 7. ガイドライン策定時の優先順位

| 優先度 | 対象 | 主に参考にする資料 |
|---|---|---|
| 最優先 | 操作方法・画面遷移・一貫性 | NN/g、Microsoft、IBM Carbon |
| 最優先 | アクセシビリティ・キーボード操作 | WCAG 2.2、WAI-ARIA APG |
| 最優先 | セキュリティ・アクセス制御 | OWASP ASVS、OWASP Top 10 |
| 高 | テーブル・検索・入力フォーム | IBM Carbon、デジタル庁、GOV.UK |
| 高 | パフォーマンス・信頼性 | Google web.dev、Microsoft Azure Architecture |
| 高 | テスト・監視 | Playwright、Google SRE |
| 中 | 拡張性・保守性・利用分析 | Microsoft Architecture、MeasuringU |

## 8. 独自ガイドラインに落とし込むための推奨フォーマット

| フィールド | 記述例 |
|---|---|
| 分類 | UX・操作性 |
| ガイドライン名 | 複数選択操作 |
| 原則 | ユーザーが複数項目を効率的に選択できること |
| 標準仕様 | チェックボックスによる複数選択を提供する。PCではCtrl・Shift操作にも対応することを検討する |
| 例外・注意点 | タッチ操作では修飾キーに依存しない。選択状態とフォーカスを区別する |
| 適用範囲 | データテーブル、ファイル一覧、ツリー |
| 参考資料 | Microsoft Selection Modes、WAI-ARIA Listbox |
| 確認方法 | マウス、キーボード、タッチ操作で選択・解除できることをテストする |

**推奨：** Webアプリ全体の設計基準を整備する場合は、最初に **No.1〜14とNo.15〜16** の16項目を詳細化し、操作パターン単位で仕様とテスト方法を定義すると効果的です。