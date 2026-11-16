# Page Navigation — Review & Acceptance Test Matrix

作成日: 2026-11-16 / [UX規約](../design/page-navigation.md) / [実装規約](../architecture/routing-navigation.md)

目的: ページ遷移を「見た目だけでなくURL・履歴・アクセシビリティ・データ・セキュリティ・計測」まで検証する。
各案件でケース番号と結果を `docs/project/` のQA記録へ反映する。**利用できない実行環境では未実施と明記し、合格とは扱わない。**

## 1. Route / Navigation Test Matrix

| ID | テスト | 期待結果 | 優先 |
| --- | --- | --- | --- |
| N-01 | 主要ナビゲーションからページへ | 正しいURL、H1、タイトル、選択状態。移動先が理解できる | 必須 |
| N-02 | URL直接入力 / リロード | SSR/RSC初期表示が成立し、認証や404が正しい | 必須 |
| N-03 | 新規タブ / 右クリック / リンクコピー | 意味のある `href` により機能する | 必須 |
| N-04 | ブラウザ戻る → 進む | 期待したページ・画面状態に復帰。履歴が無駄に積まれない | 必須 |
| N-05 | 一覧 → フィルタ/ソート/ページ → 詳細 → 戻る | 条件と可能な範囲のスクロールが復元される | 一覧あり |
| N-06 | アンカー / 固定ヘッダー | 見出しが隠れず、目標位置へ移動できる | 対象あり |
| N-07 | キーボードのみで主要導線を使用 | Tab/Enter/Shift+Tab/必要に応じてEscが動く、フォーカス可視 | 必須 |
| N-08 | 画面リーダーでのルート変更 | `<title>`、H1、landmark、current pageが適切。フォーカス喪失なし | 必須 |
| N-09 | モバイル・デスクトップメニュー | 項目位置/名称が一貫し、開閉・タッチ・復帰が動く | 対象あり |
| N-10 | 低速回線 / RSC読み込み | 反応が明確で、ロード中も操作が破綻しない | 必須 |
| N-11 | 通信断・APIエラー・キャンセル | リトライ/戻る/安全な次の操作ができる | 必須 |
| N-12 | 存在しないURL / 権限外URL | 404/403等の適切な案内と権限保護。必要なSEO/HTTPも確認 | 必須 |
| N-13 | Googleログイン/匿名ユーザー → 保護画面 | 期待した許可済み内部URLに復帰、ループや外部誘導なし | 認証あり |
| N-14 | 外部リンク / OAuthコールバック | 行き先が明確、コールバック検証、安全なエラー復帰 | 外部連携あり |
| N-15 | 編集途中で戻る/移動/再読込 | 案件の保存/破棄/下書き要件どおり。データが意図せず消えない | 編集あり |
| N-16 | ページ遷移を高速連打 | 古いデータの表示や二重送信/誤動作なし | 必須 |
| N-17 | 遷移演出あり/なし・reduced motion | 非対応でも移動可能、低減設定が尊重され、焦点・スクロールが安全 | 演出あり |
| N-18 | 公開ページのSEO | `<a href>`、title、canonical/robots等が用途に合い、初期HTMLを検証 | 公開コンテンツあり |
| N-19 | GA4 SPAページビュー | 初回+各仮想ページで1件を想定し、重複/欠落と参照元をDebugViewで確認 | GA4あり |
| N-20 | Cloudflare Web Analytics | SPA自動計測の実際の動作を確認し、カスタム計測との重複を防止 | CF Analyticsあり |
| N-21 | Cloudflareプレビュー/本番相当のルート | RSC、共有レイアウト、`next/link`、プリフェッチ、404/戻るが環境ごとに動く | 必須 |
| N-22 | 視覚・操作の一貫性 | 現在地/読み込み状態/戻り先/メニューラベルが主要ページで一貫 | 必須 |

**必須**はこの共有標準のデフォルト受け入れ条件であり、WCAG規格の正式な成功基準や法律における義務を意味しません。適用範囲・優先度・例外は案件の要求に記載してください。

## 2. Playwrightのテスト例（既存アプリ向け）

ルート名や要素名は例です。プロジェクトの実ルートとURLパラメータに合わせて編集してください。テスト環境に実行可能なVinextアプリ・Playwright設定がある場合のみ使用します。

```ts
import { test, expect } from "@playwright/test";

test("direct URL, link navigation, back/forward", async ({ page }) => {
  await page.goto("/items?page=2");
  await expect(page).toHaveURL(/\/items\?page=2/);
  await expect(page.getByRole("heading", { level: 1 })).toBeVisible();

  // Real app must expose a link with accessible text.
  await page.getByRole("link", { name: "詳細を開く" }).first().click();
  await expect(page).toHaveURL(/\/items\/[^/?#]+/);

  await page.goBack();
  await expect(page).toHaveURL(/\/items\?page=2/);

  await page.goForward();
  await expect(page).toHaveURL(/\/items\/[^/?#]+/);
});
```

- 実DOMにアクセスできるテストを中心に、必要に応じて手動で履歴スクロールとアクセシビリティも検証。
- `page.waitForURL()` と `expect(page).toHaveURL()` 等を用途に応じて利用し、無条件の固定待ち時間を使わない。
- PlaywrightにはBFCache復帰テストの制約があるため、**実ブラウザで戻る/進むの挙動を追加確認**する。

## 3. 手動テストと記録の手順

1. 主要なユーザージャーニーを3つ程度選び、画面・URL・戻り先を文章で列挙。
2. マウスだけでなくキーボード、タッチ、可能なら画面リーダーで一通り実行。
3. 通常回線と低速回線、モバイルとデスクトップ、reduced-motion設定の両方を検証。
4. 未保存データの離脱、ログイン復帰、403/404、外部URL、過去の履歴からの戻りを検証。
5. GA4はDebugView、Cloudflareは実際のSPA計測結果とNetwork等を確認。
6. 結果を `Pass / Fail / Not tested / Not applicable` に分類し、根拠（URL、操作、スクリーンショット/動画、環境、コミット）を記録。
7. 修正後に再実行し、テスト不能項目は理由を明示する。

### レポート記録例

| ID | 状態 | 環境 | 実行内容・証拠 | 課題 / 次のアクション |
| --- | --- | --- | --- | --- |
| N-01 | Not tested | － | － | デプロイ後に主要導線を確認 |
| N-08 | Not tested | － | － | NVDA/VoiceOverの手動試験を追加 |
| N-19 | Not applicable | － | GA4未導入 | － |

## 4. 根拠資料

- [Playwright Navigations](https://playwright.dev/docs/navigations) — URL遷移とBFCacheテスト上の注意点
- [WAI-ARIA APG](https://www.w3.org/WAI/ARIA/apg/) — ナビゲーション・キーボード操作
- [WCAG 2.2](https://www.w3.org/TR/WCAG22/) — 適用されるアクセシビリティの成功基準
- [Google Analytics SPA Measurement](https://developers.google.com/analytics/devguides/collection/ga4/single-page-applications) — SPAページビュー検証
- [Cloudflare Web Analytics for SPAs](https://developers.cloudflare.com/web-analytics/get-started/web-analytics-spa/) — SPA計測の自動処理
- [Vinext](https://github.com/cloudflare/vinext) — ルーター互換性とデプロイ差分
