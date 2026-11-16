# Webサイトのナビゲーション・ページ遷移設計標準

作成日: 2026-11-16 / 標準バージョン: v1.0（共通開発標準 v1.6）
対象: 日本語・英語混在のWebサイト、SaaS、データ一覧・検索・編集を含むWebアプリ。
技術構成: React / Vinext (Next.js-compatible App Router) / RSC / Cloudflare。**設計原則はフレームワークに依存しません。**

## 目的と適用

ページ遷移は「画面を切り替える効果」だけではありません。ユーザーが **現在位置、次の行き先、移動結果、戻り方** を理解でき、URL・履歴・読み込み・フォーカスが一貫して機能することを目標にします。

- **既定**: 確実に動作するシンプルな遷移。不要な演出を追加しない。
- **アクセシビリティ**: プロジェクトで決めたWCAG目標（2.2 AAを候補）に照らして検証する。規格上の必須事項と推奨設計を混同しない。
- **優先順位**: セキュリティ・要求・操作可能性 > 情報設計と履歴 > 速度とフィードバック > 任意のアニメーション。
- **既存ガイドラインとの関係**: 元の[30項目の原文](ui-guidelines.md)は変更せず、特にNo.2–6・8–9・13・15–18・20・24–27・30をページ遷移に具体化した補足規約です。
- **実装手順**: [Vinext / RSCでのルーティング](../architecture/routing-navigation.md)。**検証**: [ナビゲーションのレビュー手順](../workflows/navigation-review.md)。
- **プライバシーポリシーへの導線**: 案件に適用される[プライバシー記述標準](../legal/privacy-policy-standard.md)を確認し、公開済みポリシーのリンクをフッター、ログイン/登録、個人データを取得するUIなどに適切に設置。ログイン不要の直接URLで閲覧できるようにする。

## 1. 設計の初期成果物（必須）

サイト・アプリの制作前に、案件の `docs/project/brief.md` または対応ドキュメントに次を記録します。

1. **サイトマップ・ルート表**: URLパス、画面名、目的、主な入口、遷移先、公開範囲、SEO方針、認証要否。
2. **主要ユーザーフロー**: 例「一覧 → 絞り込み → 詳細 → 編集 → 保存 → 詳細」「Googleログイン → 元のページへ復帰」。
3. **ナビゲーション構造**: グローバル、ローカル/サイド、パンくず、フッター、モバイル。どの画面で表示/非表示か。
4. **状態と履歴**: URLに保存する検索条件・ページ番号・タブ、履歴 push/replace、戻り先・スクロール・未保存データの扱い。
5. **画面状態**: ローディング、データなし、成功、エラー、404、403、期限切れ、通信断、キャンセル。
6. **UX検証基準**: ユーザーが迷わず目的地に到達できるか、キーボード・画面リーダー・モバイル・低速ネットワークで動くか。

### ルート定義の記録フォーマット

| Path / Route | 画面・目的 | 公開・保護 | 主な入口と出口 | 状態と履歴 | title / H1 | フォールバック |
| --- | --- | --- | --- | --- | --- | --- |
| `/search?q=...` | 検索結果 | 公開/案件次第 | トップ → 検索 → 詳細 | 検索条件をURL化 | 検索結果 | 読込・0件・失敗 |
| `/items/[id]` | 詳細 | 案件次第 | 一覧 → 詳細 → 編集 | 戻ると元の一覧条件 | 詳細タイトル | 404/403 |
| `/login?returnTo=...` | ログイン | 公開 | 保護画面 → 認証 → 元の画面 | 検証した内部パスのみ | ログイン | エラー・中断 |

※ これは**記述例**であり、ルートファイルや認証実装を作成したわけではありません。

## 2. 情報アーキテクチャとナビゲーション

| ID | 基準 | 設計・受け入れ観点 |
| --- | --- | --- |
| NAV-01 | 利用者のタスクを中心にルートを分類 | コンテンツ、業務、検索、設定、アカウントを実際のニーズに従って整理。過度に深い階層や内部用語を避ける |
| NAV-02 | 主要ナビゲーションの一貫性 | 同等のページでラベル・並び・位置を揃える。狭い画面でも項目名と順序を不必要に変えない |
| NAV-03 | 現在地が把握できる | ページタイトル/H1、現在項目の視覚表示と必要な `aria-current="page"`、適切な見出し構造 |
| NAV-04 | リンクの意味が分かる | 「詳細」だけでなく、文脈やアクセシブルネームからリンク先を理解できるラベル |
| NAV-05 | 階層に応じた補助ナビ | パンくずは階層構造がある場合に採用。作業フローの「戻る」と階層を示すパンくずは目的が異なる |
| NAV-06 | ナビゲーションを隠しすぎない | PCの主要機能は見つけやすく配置。モバイルの開閉メニューはボタン、展開状態、フォーカス管理を備える |

- 通常のWebサイトのドロップダウンは、まず **HTMLのリンク + 開閉ボタン（disclosure）** を検討する。WAI-ARIA `menu`/`menubar` ロールは、アプリ型の複合ウィジェットのキーボード規則が必要な場面だけ使う。
- パンくずには名前の付いた `<nav>`、階層順のリストを使う。現在ページがリンクなら `aria-current="page"` を検討。
- スキップリンク（本文へ）、主ナビゲーション、サブナビゲーションなどのランドマークを整理。メニュー操作はタッチ、キーボード、画面リーダーで確認。

## 3. リンク、操作とURL

| ID | 基準 | 設計・受け入れ観点 |
| --- | --- | --- |
| NAV-07 | **移動はリンク、操作はボタン** | 別URLの画面へ移動 → `<a href>` / `Link`。保存・削除・開閉 → `<button>`。クリック専用 `div` をリンクにしない |
| NAV-08 | 深いリンクと直接アクセス | すべての共有可能な画面はURLを開く、リロードする、別タブで開く操作が機能する |
| NAV-09 | URLが意味を持つ | 安定したパス、適切なリソースID、必要に応じて `?q=`・`?sort=`・`?page=` を活用。内部秘密情報をURLに含めない |
| NAV-10 | URLと画面を同期 | 検索/絞り込み/並び替え/ページ番号など共有・復帰したい状態をURLに表現。入力途中や一時ホバーは必要な場合を除きURL化しない |
| NAV-11 | 予測可能な外部リンク | 必要なく新規タブを強制しない。新規タブは明示し、外部サービスへ移ることが分かるラベルにする |

- **履歴に残す重要な移動**は push、正規化やURLの軽微な訂正は replace を検討。キー入力ごとに履歴を増やさない。
- サイト内リンクはマウス中ボタン/修飾キー/リンクコピー/新規タブが機能すること。
- 公開ページの重要リンクはクロール可能な `<a href="...">` を使う。

## 4. 戻る・進む、スクロール、フォーカス

| ID | 基準 | 設計・受け入れ観点 |
| --- | --- | --- |
| NAV-12 | 履歴を壊さない | ブラウザの戻る・進むで意味のある直前状態へ復帰。戻るボタンが前のページと無関係な画面へ飛ばない |
| NAV-13 | 戻ると作業に復帰できる | 一覧 → 詳細 → 戻るで検索条件、ページ番号、可能な範囲のスクロール位置・選択状態を復元 |
| NAV-14 | スクロール位置が意図通り | 新規ページは上部/メインコンテンツから読める。履歴移動は可能な範囲で以前の位置へ。固定ヘッダーのアンカーずれに配慮 |
| NAV-15 | ルート変更を認識できる | ドキュメントタイトル・現在地・主見出しを更新。SPA/RSC遷移後にフォーカスが消えないことを検証し、必要に応じて新しいH1/本文領域へ管理されたフォーカス移動 |
| NAV-16 | 未保存データの保護 | 編集中の離脱やブラウザ戻るに伴うデータ損失を防ぐ設計（保存/破棄/キャンセル、下書き/自動保存など）を案件で明示。必要な範囲だけ離脱警告 |

- 新しいページへの移動と同じ画面内のタブ切り替えで、**フォーカスやスクロールを一律に強制しない**。意図するユーザーフローと入力方式に基づいて選択。
- `history.scrollRestoration = "manual"` を安易に全アプリへ設定しない。まずブラウザとVinextの標準復元に委ね、必要な場合にのみ制御。
- 戻り先が不明な場面（外部サイトからの直接訪問等）では、案件で定めた安全な親URLへのリンクを提供。履歴の有無だけで自サイトに戻れるとは判断しない。

## 5. 状態表示とレイアウトの継続性

| ID | 基準 | 設計・受け入れ観点 |
| --- | --- | --- |
| NAV-17 | 遅延時にも反応が分かる | サーバー処理やRSC遷移にLoading UI / Suspenseを設け、利用者に反応中であることを伝える |
| NAV-18 | 共有レイアウトを安定化 | 不要な再描画でナビゲーションが消えたり、ヘッダーやサイドバーの位置が飛んだりしない |
| NAV-19 | エラーから復帰できる | 404/403/期限切れ/通信失敗は理由と次の行動（再試行、一覧へ、ログイン等）を示す。成功時も行き先・結果を示す |
| NAV-20 | ナビゲーションが操作不能にならない | 連続クリック、通信遅延、キャンセル、戻る操作、オフライン復帰等で二重送信や状態の矛盾が起きないよう検証 |

- `loading.tsx` や `Suspense` の実際の発火位置は、Vinextバージョンや共有レイアウト構成ごとに検証する。
- ローディング中もページ全体を不必要にブロックしない。処理状態の読み上げは必要に応じて控えめな `role="status"` などを使う。
- 破壊的操作は移動とは切り離し、必要な確認・Undoを用意する。

## 6. ページ遷移アニメーション（任意）

- **デフォルトはアニメーションなし**。コンテンツの階層、詳細への遷移、ギャラリーの位置関係など、理解を助ける場合のみ採用。
- [View Transition API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API)は同一ドキュメント内の状態変化、同一オリジンの別ドキュメント遷移で用途が異なる。RSC / Vinextルーターと二重に遷移を制御しない。
- **必ずプログレッシブエンハンスメント**：非対応ブラウザではアニメーションなしでも機能する。
- `prefers-reduced-motion: reduce` の場合は大きなスライド/ズームなどの視覚運動を止めるか、控えめな表現に変更する。アニメーション終了を移動の成立条件にしない。
- 原則は短く穏やかに（例: 150–250msを初期検討値として実機で評価。**規格上の閾値ではない**）。過剰な大画面移動、ページ全体の常時ワイプ、演出を待たせる遷移を避ける。
- 最終レイアウトやフォントが確定する前に旧/新画面をモーフィングすると違和感が生じる。フォント・画像・レイアウトの安定性も評価する。

### 参考CSS（導入する場合のみ）

```css
/* Cross-document, same-origin MPA navigation only.
   Add to both source and destination; not an automatic SPA/RSC solution. */
@media (prefers-reduced-motion: no-preference) {
  @view-transition { navigation: auto; }
}

@media (prefers-reduced-motion: reduce) {
  ::view-transition-group(*),
  ::view-transition-old(*),
  ::view-transition-new(*) { animation: none !important; }
}
```

**注意**：SPAのView Transition API連携は実装方法が異なります。ルーター、フォーカス、スクロール、対応ブラウザを検証するまで追加しないこと。

## 7. よくある画面遷移の設計例

| フロー | 期待する動作 |
| --- | --- |
| 検索結果 → 詳細 → 戻る | フィルタ・並び順・ページとスクロールが可能な範囲で復元 |
| 編集 → 保存成功 | 成功を明示。詳細へ移動するか同じ画面に残るかを案件で確定。二重送信なし |
| 編集中 → 離脱 | 未保存内容の保護・保存・破棄・中断が要件に沿って扱われる |
| 保護画面 → Googleログイン → 元の画面 | OAuthコールバックを検証し、許可された戻り先へ遷移。リダイレクトループなし |
| 匿名利用 → ログイン | 必要ならデータ/作業状態を失わず引き継ぐ |
| 存在しないURL | 404相当の表示、存在する近接ページや一覧への導線 |
| 権限外URL | データを出さずに403/ログイン等の適切な案内 |
| スマートフォンでメニュー開閉 | 表示/非表示、フォーカス、閉じた後の操作、戻る・画面回転を検証 |

## 8. 根拠資料

- [NN/g Menu Design Checklist](https://media.nngroup.com/media/articles/attachments/PDF_Menu-Design-Checklist.pdf) — 見つけやすいナビゲーション、現在地、リンクラベル（推奨）
- [WCAG 2.2 — 3.2.3 Consistent Navigation](https://www.w3.org/WAI/WCAG22/Understanding/consistent-navigation) — 繰返しナビゲーションの順序（Level AA）
- [WCAG 2.2 — 2.4.3 Focus Order](https://www.w3.org/WAI/WCAG22/Understanding/focus-order) — 意味を保つフォーカス順（Level A）
- [WCAG 2.2 — 3.2.1/3.2.2 On Focus/On Input](https://www.w3.org/TR/WCAG22/#predictable) — 予期しないコンテキスト変更への注意
- [W3C APG Disclosure Navigation](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/) / [Breadcrumb Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/) — 操作・現在地の実装例
- [WAI Menu Structure](https://www.w3.org/WAI/tutorials/menus/structure/) — メニュー構造と現在項目
- [Google Search Central — Link Best Practices](https://developers.google.com/search/docs/crawling-indexing/links-crawlable) — 実リンクとアンカーテキスト
- [GOV.UK — Back Link](https://design-system.service.gov.uk/components/back-link/) — 作業フローの戻り先
- [MDN — History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API) / [Scroll Restoration](https://developer.mozilla.org/en-US/docs/Web/API/History/scrollRestoration) — ブラウザ履歴と位置復元
- [MDN — View Transition API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API) / [prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/%40media/prefers-reduced-motion) — 任意の遷移演出
- [Next.js — Linking and Navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating) — RSC / Link / プリフェッチ / Streaming
- [Vinext — compatibility](https://github.com/cloudflare/vinext) — バージョン依存の実装確認

本資料の推奨方針は、各資料の技術要件と実務判断を統合した**プロジェクト標準**です。W3C等がここに記したすべてを義務づけているという意味ではありません。
