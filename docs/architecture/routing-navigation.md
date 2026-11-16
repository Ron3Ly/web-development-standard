# Vinext / RSC / Cloudflare — Routing & Navigation Implementation Standard

作成日: 2026-11-16。参照: [ページ遷移UX](../design/page-navigation.md) / [受け入れテスト](../workflows/navigation-review.md) / [既存30項目](../design/ui-guidelines.md)。

**この文書はアプリの実装規約です。実際のVinextバージョン、パッケージ構成、App Router機能のサポート範囲を必ず確認してください。** Next.jsと同一の挙動を無条件には保証しません。

## 1. ルーティング方式とルートの契約

- VinextのApp Router（プロジェクトで採用している場合）を優先し、既存の `app/` の構造と `next/*` 互換APIに従う。
- 新規URLの設計前にルート一覧、アクセス制御、メタデータ（title/description/canonical）、404/403、エラー画面、ローディング、共有レイアウトを定める。
- 一覧 `/items` と詳細 `/items/[id]`、編集 `/items/[id]/edit` のようにパスを一貫させる。実際の階層は案件のサイトマップに従う。
- 直接URLアクセス・再読み込み・新規タブでもページが成立することを確認。保護されたデータはWorkers/サーバー側で権限を検査。
- `generateMetadata` 等の互換API、サーバーリダイレクト、`notFound()`、`loading.tsx`、`error.tsx` が実際にどう動くかをビルドおよびCloudflareプレビュー/本番相当環境で検証する。

### 導線の種類と使う手段

| 意図 | 第一候補 | 留意点 |
| --- | --- | --- |
| 同一サイトのページ移動 | `<Link href="/...">` from `next/link` | 最終的にリンク要素になること。バージョン依存のprefetch/scroll挙動を検証 |
| 外部Webサービスへ移動 | 普通の `<a href="https://...">` | 新規タブは必要な場合に限り説明。必要なら`rel="noopener noreferrer"` |
| 保存・送信・削除・メニュー開閉 | `<button>` / `<form>` | GET/POSTの意味を守る。URL移動と混同しない |
| 保存成功後などのコードによる遷移 | 検証済みの `useRouter().push/replace` またはサーバーの `redirect` | 更新・保存の完了確認後。重複処理防止 |
| 同一ページ内の見出しへ移動 | `<a href="#section-id">` | `scroll-margin-top` などで固定ヘッダー回避 |
| 共有できる検索・絞り込み | URL検索パラメータ | URLパラメータの正規化・型検証・エンコード |

### コンポーネント例（概念; React / TypeScript）

```tsx
// App Router / Vinext のバージョンで動作確認が必要
import Link from "next/link";

export function ItemLink({ id, title }: { id: string; title: string }) {
  return <Link href={`/items/${encodeURIComponent(id)}`}>{title}</Link>;
}
```

`href` に外部入力をそのまま通さず、固定の内部パス構造と妥当なIDを組み合わせる。データ由来のリダイレクト先は別途厳格に検証する。

## 2. 現在地・リンク・アクセシブルなナビゲーション

- `<nav aria-label="メインナビゲーション">` 内に明確なリンクを置く。リンク先は実際の `href` を持つ。
- 現在ページへのリンクに `aria-current="page"`、見た目にも選択表示。現在のパス取得には検証済みの `usePathname()` 等を使う。
- 親階層のセクションを示す場合、現在ページと同じARIA状態を安易に流用しない。
- メニュー開閉は `<button aria-expanded aria-controls>`、開閉後のフォーカスとEsc等の必要な操作を設計。一般的なサイトナビに `role="menu"` を不要に付けない。
- パンくずは `<nav aria-label="パンくずリスト"><ol>...` のように意味のある構造で表示する。
- キーボードのTab/Shift+Tab、Enter、目に見えるフォーカス、スキップリンク、スマートフォンのタッチ操作を検証する。

```tsx
"use client";
import Link from "next/link";
import { usePathname } from "next/navigation";

const links = [
  { href: "/", label: "ホーム" },
  { href: "/items", label: "一覧" },
  { href: "/settings", label: "設定" },
];

export function PrimaryNav() {
  const pathname = usePathname();

  return (
    <nav aria-label="メインナビゲーション">
      <ul>
        {links.map(({ href, label }) => (
          <li key={href}>
            <Link href={href} aria-current={pathname === href ? "page" : undefined}>
              {label}
            </Link>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

**注意:** この例は静的な完全一致パスのみ。動的セグメント、basePath、ロケール、末尾スラッシュや「一覧配下の詳細」は案件のルート定義に合わせて判定を作る。

## 3. ブラウザ履歴・URL状態

- `push`: 新たに戻る価値のある画面・重要な状態の遷移。
- `replace`: 正規URLへの書換えや頻繁な補助状態の変更。重大な遷移履歴を消す目的で使わない。
- `back()`: 前の履歴が**このサイトの画面とは限らない**。独自の戻るUIは既知の親URL・検証済みの `returnTo` など安全な代替先を設計する。
- リストの `q`、`filter`、`sort`、`page` は必要に応じてURLへ。ユーザーが戻ったときに復元できるようにする。
- 資格情報、OAuthアクセストークン、機密のユーザー情報はURLに含めない。
- DOM上の状態を更新せずにURLだけを変更してはいけない。ルーターと同期するAPIを使用し、初期表示と戻る/進むをテスト。
- `history.scrollRestoration` は原則 `auto` を維持し、既定のブラウザ/ルーター復元と競合する手動処理を入れない。スクロール固定やタブ遷移等は必要なものだけ設定。

## 4. 画面が切り替わったときの認識

- ルートごとに意味のある `<title>`、主見出しと画面内容を設定し、SPAのクライアント遷移後も更新されることを確認。
- 新しい「ページ」と認識させる必要がある場合、レンダリング完了後に新しいH1やメイン領域へ適切にフォーカスを移す（例: `tabIndex={-1}`）。フォーカスが消える、画面外へ飛ぶ、二重に読まれる等を防ぐ。
- ただしタブ・モーダル・履歴戻りでは一律H1へフォーカスしない。操作モデル、現在のフォーカス、実装するコンポーネント、ユーザーテストに基づき判断。
- データ更新通知は過剰なライブリージョン連発を避け、状態変化の意味が伝わるようにする。

## 5. RSC・遅延読み込み・プリフェッチ

- Server Components主体のレンダリングを保持。ユーザー操作が必要なナビコンポーネントのみ適切にClient Component化。
- `loading.tsx` / `Suspense` で**意味のある待機状態**を設計し、共有レイアウトを必要以上に再描画しない。
- プリフェッチはルートやフレームワークが扱うものを利用し、全リンクの手動プリフェッチは行わない。費用の高いD1参照・認証済み個人データを含むページは、漏えい、コスト、キャッシュ境界、リクエスト数を評価する。
- プリフェッチはGETで副作用を起こさない。保存/課金/削除などはクリックによる意図した操作だけで実行する。
- 最新のVinext互換性資料では `next/link` と `next/navigation` の対応が記載されているが、loading/fallback/共有レイアウト/スクロール等は特定のバージョンで差分があり得る。**プロジェクトのバージョン固定・実測を優先**。
- 遅いネットワークでリンクを押した直後にUIが無反応に見えないか確認。二重クリック時の安全性も検証。

## 6. 404/403、認証リダイレクト、外部リンク

- 存在しないパスとアクセス権なしのパスを区別。情報漏えいに配慮し、適切なエラー画面と一覧/ホーム/再ログインなどの安全な行き先を示す。
- **サーバー側で必ず認可**。ナビ項目の表示/非表示だけでは権限管理にならない。
- `returnTo` 等は検証済み内部パスかサーバー管理の行き先IDに限定する。外部ドメイン・`//evil.example`・意図しないスキーム等を拒否し、Open Redirectを防ぐ。
- OAuthコールバック、ログイン完了後の戻り先、匿名→ログインのデータ引継ぎ、ログイン失敗/中断、権限変更、認証期限切れを設計する。
- サードパーティのSpotify/Apple Music/YouTube等への遷移は外部サービス名や結果が分かるUIにし、OAuth権限とコールバックURLを最小化。
- ストリーミング中のHTTPステータス/404制御はプラットフォーム・実装に依存。HTMLの表示だけでなく実際のレスポンスとインデックス制御を検証する。

## 7. SEO・メタデータ・分析

- 公開コンテンツの重要な導線は `<a href>` を持つクロール可能なリンク。JS `onClick` だけのdiv等を主要導線にしない。
- ページ別タイトル、説明、canonical、インデックス/非インデックス、サイトマップを必要に応じて設定し、直接アクセスしたHTMLを確認する。
- **GA4:** SPAの仮想ページ遷移は設定に応じてブラウザ履歴ベースの自動計測または手動 `page_view` を選ぶ。GTMの履歴トリガーとGA4自動履歴計測を同時使用して二重計測しない。DebugViewで `page_location` / `page_referrer` を確認。
- **Cloudflare Web Analytics:** 現行ドキュメントではSoft Navigations API、Navigation API、History APIによるSPA自動計測を説明している。標準導入時は自動計測の有無を確かめ、独自計測で重複させない。
- GA4とCloudflareの双方を使う場合、各製品でのページビュー定義・除外パス・同意・ユーザー識別を確認。実際の転送データを検査し、個人情報をイベントURLに載せない。

## 8. View Transition API: 任意のプログレッシブ拡張

- 本体の移動はアニメーションなしでも正常に成立する。
- 同一ドキュメント内は `document.startViewTransition()` の利用可否とルーター同期が必要。別ドキュメント間では同一オリジン・双方の `@view-transition` 指定など別の条件。
- **React/Vinextの標準ルーティングと二重にhistory/DOMを書き換えない**。トランジションは移動・データ取得・フォーカス設定の責務を置き換えない。
- `prefers-reduced-motion` を尊重し、非対応環境では静的に切り替わる。初回表示、戻る操作、フォント/画像読み込み、低速環境、スクリーンリーダーを確認。
- デフォルトの演出なし、または短い視覚フィードバックを推奨。すべての画面に大がかりなスライド演出を強制しない。

## 9. テスト・レビュー

[Navigation Review](../workflows/navigation-review.md) の検証表に基づき、少なくとも以下を確認:

- 新規タブ/深いURL・再読み込み・戻る/進む・スクロール復元・検索条件復元
- キーボード・画面リーダー・現在地表示・タイトル・フォーカス
- モバイルナビ・オーバーレイ・遷移と連続クリック
- ローディング/空/エラー/404/403/ログインの回復導線
- reduced motion・View Transition非対応時の機能
- SEOのHTMLリンクとページタイトル、実際のHTTPステータス
- GA4/Cloudflare SPAページビューの過不足・重複
- Cloudflare Workersの本番相当環境での動作

## 一次資料（実装時に再確認）

- Vinext docs: https://vinext.dev/docs
- Vinext source / compatibility: https://github.com/cloudflare/vinext
- Next.js Linking and Navigating: https://nextjs.org/docs/app/getting-started/linking-and-navigating
- Next.js Link API: https://nextjs.org/docs/app/api-reference/components/link
- Next.js Loading: https://nextjs.org/docs/app/api-reference/file-conventions/loading
- WAI-ARIA APG Navigation Example: https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/
- WAI-ARIA APG Breadcrumb: https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/
- WCAG 2.2: https://www.w3.org/TR/WCAG22/
- Google SEO links: https://developers.google.com/search/docs/crawling-indexing/links-crawlable
- MDN History API: https://developer.mozilla.org/en-US/docs/Web/API/History_API
- MDN ScrollRestoration: https://developer.mozilla.org/en-US/docs/Web/API/History/scrollRestoration
- MDN View Transitions: https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API
- MDN prefers-reduced-motion: https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/%40media/prefers-reduced-motion
- OWASP Open Redirect: https://cheatsheetseries.owasp.org/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.html
- Google GA4 SPA: https://developers.google.com/analytics/devguides/collection/ga4/single-page-applications
- Cloudflare Web Analytics SPA: https://developers.cloudflare.com/web-analytics/get-started/web-analytics-spa/
