# 品質測定・テストプレイブック

**2026-11-17** / [品質標準](quality-standard.md) / [品質目標](quality-targets.template.md) / [レビュー](../workflows/quality-review.md)。

実行可能なWebアプリのある**案件リポジトリでのみ**コマンドやテストを実行する。この共通標準リポジトリにはVinextアプリやテストランナーは含まない。

## 1. 準備
1. 公開/ログイン/ゲスト/検索/データ詳細/保存/404/外部APIの代表URLを選ぶ。
2. 使用端末、ブラウザ、低速回線、言語とテストデータ/ロールを固定。Project briefと[品質目標](quality-targets.template.md)へ記載。
3. 各テストにルート・操作・期待結果・測定方法・証拠・責任者を紐付ける。

## 2. Playwright E2E
[公式Best Practices](https://playwright.dev/docs/best-practices)の**ユーザーに見える振る舞い**、`getByRole`/`getByLabel`などのロケータ、分離されたテストデータとweb-first assertionsを採用。外部サービスの不安定な応答はテスト用スタブで制御する。

**概念例。実アプリのURL/ラベルに書き換え、Playwrightを導入した案件でのみ使用：**

```ts
import { test, expect } from "@playwright/test";

test("search -> detail -> back", async ({ page }) => {
  await page.goto("/items?q=music&page=2");
  await expect(page.getByRole("heading", { level: 1 })).toBeVisible();
  await page.getByRole("link", { name: "詳細を開く" }).first().click();
  await expect(page).toHaveURL(/\/items\/[^/?#]+/);
  await page.goBack();
  await expect(page).toHaveURL(/\/items\?q=music&page=2/);
});
```

操作成功だけでなく0件・無効入力・Googleログイン中断・セッション期限切れ・匿名ユーザー昇格・R2失敗・API 429・失敗後の再試行を、該当案件で検証。

## 3. Lighthouse CI（ラボ）
[Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci)は回帰測定/アサーションを支援。対象URL、端末・ネットワーク・測定回数・環境を記録する。CIの`warn`と`error`は案件別予算に従う。スコア90点をプロジェクトの初期参考値にしても、**自動的に全案件の必須条件とはしない**。結果のばらつきがあるため複数回計測する。

例（`lighthouserc`の一部。依存・設定・実際の対象アプリは別途必要）：

```js
{
  ci: {
    assert: {
      assertions: {
        "categories:performance": ["warn", { minScore: 0.9 }]
      }
    }
  }
}
```

## 4. Core Web Vitals（フィールド）
[PSI/CrUX](https://developers.google.com/speed/docs/insights/v5/about)では通常**直近28日**の実ユーザーデータ。LCP p75≤2.5s、INP p75≤200ms、CLS p75≤0.1がGoogleの良好区分。ページ固有のサンプル不足でオリジン集計となること、両者とも測定不能なこともある。未計測なら**Insufficient data**。

ラボのLighthouseスコアはフィールドCWVや実操作INPの代わりではない。公開前はラボ＋実端末、公開後は実ユーザーデータで傾向を判断。CWV p75の値、期間、対象URL/端末を必ず記録。

## 5. WCAG・ブラウザ・国際化
WCAG 2.2 AAは標準的な検討目標。法・契約の適用要件が優先。2026年公開の[WCAG-EM 2.0](https://www.w3.org/TR/WCAG-EM/)（**W3C Group Note、追加の適合要件ではない**）を参考に評価対象/サンプル/結果を記録。自動検査では全ての問題を判定できないので、キーボード・支援技術・ズーム・動的状態の手動チェックも実施。

[Baseline](https://web.dev/baseline)と実利用者の環境に応じてChrome/Edge/Firefox/Safari（iOS/Androidを含む）を検証し、新しいAPIは非対応時も主要操作ができるようにする。日本語/英語の`lang`、Intlで日時/時刻を確認する。

## 6. SEOと実サービス
公開コンテンツだけGoogle [Search Essentials](https://developers.google.com/search/docs/essentials)に沿ってリンク/canonical/robots/HTTP/title等を点検。ログイン専用画面を公開インデックス対象と混同しない。

Cloudflareは[Workers Observability](https://developers.cloudflare.com/workers/observability/)でLogs/Traces/Metrics/Issuesを確認。D1/R2や音楽APIの失敗時の利用者影響を確認し、OAuthトークンや個人データをログに残さない。

実行結果は**Pass/Fail/Not tested/N/A**、フィールドデータ不足は**Insufficient data**で報告。未実施テストや過去の実績を作り上げない。
