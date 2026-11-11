# Web Development Standard

GPT Work・Codex・GitHubで再利用する、Webサイト設計・実装の共通標準です。

> **このリポジトリは「仕様・指示・参考資料」の保管場所です。実行可能なWebアプリのスターターテンプレートではありません。** 各案件のリポジトリには、この標準を明示的に参照させてください。Codexは別リポジトリの `AGENTS.md` を自動読込しません。

## 標準技術スタック

| 領域 | 標準 |
| --- | --- |
| UI | React |
| フレームワーク | Vinext（Next.js互換、採用前に互換性を確認） |
| レンダリング | React Server Components（RSC）主体 |
| ビルド | Vite / Rolldown（バージョン・構成に依存） |
| デプロイ | Cloudflare Workers |
| データベース | Cloudflare D1（必要な案件で利用） |
| オブジェクトストレージ | Cloudflare R2（必要な案件で利用） |
| 認証 | Googleログイン・匿名認証（プロバイダー／実装方式は案件ごとに決定） |
| 計測 | Google Analytics 4 / Cloudflare Web Analytics |
| フォント | Geist（日本語には適切なフォールバックを指定） |
| スタイリング | CSS / ユーティリティクラス |
| UIコンポーネント候補 | [Astryx](https://github.com/meta/astryx)（React 19+、ベータ版、案件ごとに採用判断） |
| 言語 | JavaScript / TypeScript（案件ごとに決定、未確定） |

音楽関連の外部連携候補：Spotify、Apple Music、YouTube、setlist.fm、MusicBrainz、Deezer。すべてを標準搭載するという意味ではありません。

## 共通UI設計

- **主な設計指針**：[デジタル庁デザインシステム](https://design.digital.go.jp/dads/)（アクセシビリティ、情報設計、フォーム、コンポーネント）
- **UIコンポーネント基盤の候補**：[Astryx（Meta）](https://github.com/meta/astryx) — 150以上のReactコンポーネント、テーマ、CLI。デジタル庁の設計指針と併用可能。導入は任意で、Vinext / RSC / Cloudflareとの実動作検証が必要。詳しくは [Astryx利用指針](docs/design/astryx.md)。
- **UIアセット**：[Lucide Animated](https://lucide-animated.com/) / [ICOOON MONO](https://icooon-mono.com/) / [デジタル庁イラストレーション・アイコン](https://www.digital.go.jp/policies/servicedesign/designsystem/Illustration_Icons) / [freeicon.dev（Motive）](https://freeicon.dev/)
- ライセンスは出典ごとに異なります。詳細は [UIアセット使用指針](docs/design/ui-assets.md) に記載します。

## ドキュメント一覧

| パス | 内容 |
| --- | --- |
| [AGENTS.md](AGENTS.md) | AIエージェント向けの最初に読む指示 |
| [docs/architecture/tech-stack.md](docs/architecture/tech-stack.md) | 共通技術スタックと例外管理 |
| [docs/architecture/cloudflare.md](docs/architecture/cloudflare.md) | Workers・D1・R2・シークレット管理 |
| [docs/architecture/authentication.md](docs/architecture/authentication.md) | Google/匿名認証の設計判断 |
| [docs/architecture/external-integrations.md](docs/architecture/external-integrations.md) | 外部音楽サービス/API連携 |
| [docs/design/design-system.md](docs/design/design-system.md) | UI/UX・アクセシビリティ・スタイル |
| [docs/design/astryx.md](docs/design/astryx.md) | Astryxコンポーネント・テーマ・CLIの導入判断 |
| [docs/design/ui-references.md](docs/design/ui-references.md) | 参照UI・設計パターンの取り入れ方 |
| [docs/design/ui-assets.md](docs/design/ui-assets.md) | アイコン・イラスト素材と利用条件 |
| [docs/project/brief.template.md](docs/project/brief.template.md) | 案件固有要件のひな型 |
| [docs/workflows/new-project.md](docs/workflows/new-project.md) | GPT Work / Codexでの案件開始手順 |
| [.agents/skills/](.agents/skills/) | ページ設計・実装・UIレビューの手順 |

## 新しいWebサイトを作るとき

1. [案件開始手順](docs/workflows/new-project.md) に従い、案件用の作業リポジトリを用意する。
2. [案件要件テンプレート](docs/project/brief.template.md) をもとに固有要件を記載する。
3. GPT WorkまたはCodexに、このリポジトリの **AGENTS.md と関連仕様を実際に読み込ませる**。接続できない環境では、必要なMarkdownファイルを案件側にコピー・添付する。
4. 既存の技術・デザイン原則を優先し、必要な差分のみ設計・実装する。
5. UIレビュー、ビルド、アクセシビリティ、認証・API権限、計測設定を確認する。

**依頼文の例**

```text
共通開発仕様 https://github.com/Ron3Ly/web-development-standard を参照してください。
まずAGENTS.mdと、そこから指定された技術・UI関連ドキュメントを実際に読んでください。
案件固有の要件を踏まえ、Vinext / React / Cloudflare中心の構成で設計・実装してください。
不明点と互換性リスクは明示し、標準からの逸脱は理由を記録してください。
```

## 運用上の注意

- **外部リポジトリのURLだけでは自動適用されません。** 各実行環境から読めるようにして、読込指示を与えます。
- 新規案件を高速に生成するための実行可能な雛形は、別途 `web-starter` リポジトリで管理する想定です。
- Vinextは開発が進んでいるため、使用時に[公式ドキュメント](https://vinext.dev/docs)・[Cloudflareのガイド](https://developers.cloudflare.com/workers/framework-guides/web-apps/nextjs/)で対応状況を確認します。
- 案件のブランド・対象ユーザー・ライセンス・法的要件が共通のデザイン上の初期値より優先されます。

## メンテナンス

このリポジトリは文章・規約を管理します。新規依存関係や大きな設計変更は、用途・互換性・メリット・移行コストを記録してから反映してください。

最終整理：2026-11-11 / 現在版 1.2
