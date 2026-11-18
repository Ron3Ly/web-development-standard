# 2D Webゲーム向け素材ソース一覧（アイコン・音声・VFX）

調査日: **2026-11-17** / 対象: **2Dのみ**（Webブラウザ・React/Vinext/Cloudflareアプリ内ゲーム）。

このドキュメントは**公式ライセンスを確認した候補と選定ルール**を収録します。「掲載サイトのすべての素材が無条件で使える」という承認ではありません。実際のゲームで選んだ各ファイルは [素材台帳](asset-register.template.md) に登録し、[ライセンス](licensing.md)と[公開前チェック](../workflows/game-asset-review.md)を通してから利用します。

**共通標準リポジトリには、元の素材ZIP・PNG・SVG・音声ファイルをコピーしません。** サイトへの公式リンクと開発指針を管理し、実際のゲーム案件側で個別素材を採用します。

## 1. 優先して調べる素材ソース

| ソース / 配布ページ | 種類 | 公開元で確認したライセンス | 商用2Dゲーム・表示義務 / 採用条件 |
| --- | --- | --- | --- |
| [Game-icons.net](https://game-icons.net/) / [About](https://game-icons.net/about.html) | RPG装備、スキル、アイテム、ステータス用SVGアイコン | 原則 **CC BY 3.0**（一部パブリックドメイン例外あり） | 商用利用可。**個々の著作者をクレジット**。名称・作者・リンク・ライセンス・変更有無を記録 |
| [Kenney UI Pack](https://kenney.nl/assets/ui-pack) | ボタン、パネル、スライダーなど2D UI | **CC0 1.0** | 商用利用可・帰属表示不要。最終利用ファイルのライセンスを再確認 |
| [Kenney Pixel UI Pack](https://kenney.nl/assets/pixel-ui-pack) | ピクセルUI、アイコン、メニュー | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney UI Pack – Pixel Adventure](https://kenney.nl/assets/ui-pack-pixel-adventure) | ピクセルゲームのUIセット | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney Particle Pack](https://kenney.nl/assets/particle-pack) | 2Dパーティクル、エフェクト素材 | **CC0 1.0** | 商用利用可・表示不要。発光/点滅を調整 |
| [Kenney Smoke Particles](https://kenney.nl/assets/smoke-particles) | 煙・爆発の2D視覚エフェクト | **CC0 1.0** | 商用利用可・表示不要。性能と点滅を検査 |
| [Kenney UI Audio](https://kenney.nl/assets/ui-audio) | UIクリック、切り替え音 | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney Interface Sounds](https://kenney.nl/assets/interface-sounds) | メニュー、通知、操作音 | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney Impact Sounds](https://kenney.nl/assets/impact-sounds) | 衝突、打撃、フットステップ等の効果音 | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney Sci-fi Sounds](https://kenney.nl/assets/sci-fi-sounds) | SF・レーザー系効果音 | **CC0 1.0** | 商用利用可・表示不要 |
| [Kenney Music Jingles](https://kenney.nl/assets/music-jingles) | 短いジングル・ステージクリア音 | **CC0 1.0** | 商用利用可・表示不要。**長尺BGM用とは限らない** |
| [OpenGameArt](https://opengameart.org/) / [FAQ](https://opengameart.org/content/faq) | 2Dスプライト、VFX、アイコン、SFX、BGM | **作品ごと**：CC0 / CC BY / CC BY-SA / OGA-BY / GPLなど | 商用ゲームでも使える素材あり。**各投稿のライセンスと作者指定を必ず確認**。互換性に疑義がある場合は使わない |
| [Freesound](https://freesound.org/) / [FAQ](https://freesound.org/help/faq/) | UI/戦闘・自然・環境音・BGM候補 | **ファイルごと**：CC0 / CC BY 4.0 / CC BY-NC 4.0 など | CC0や条件を守ったCC BYを候補に。**NCは商用ゲームでは利用不可**。権利帰属の確認も必要 |
| [Mixkit – Game Sound Effects](https://mixkit.co/free-sound-effects/video-game/) / [Sound Effects Free License](https://mixkit.co/license/modal/sfxFree/) | ゲーム用効果音 | **独自 Sound Effects Free License** | 商用ゲーム内で利用可。**素材単体や素材集・テンプレート・ソース付き配布は禁止**。Webでの生ファイル公開形態は利用条件を確認 |
| [Sonniss – GDC 2026 Game Audio Bundle](https://gdc.sonniss.com/) / [License](https://sonniss.com/gdc-bundle-license/) | 高品質SFX、環境音、フォーリー | **独自 GDC Bundle License** | 商用ゲーム/インタラクティブ作品で利用可・クレジット不要。**単体販売不可、AI学習・開発への利用禁止**。音声の保存・配布方式を要確認 |
| [itch.io – 2D game assets](https://itch.io/game-assets/tag-2d) / [Commercial Use tag](https://itch.io/game-assets/tag-2d/tag-commercial-use) | 2D/UI/ピクセル/VFX/音声 | **各出品者・各パック固有の条件** | タグに商用利用可とあっても、**個別のライセンス文書・著作者・再配布条件**を確認してから採用 |

確認元: Kenneyの[公式Support](https://kenney.nl/support)は資産ページのゲーム素材をCC0と明記。Game-iconsの[公式FAQ](https://game-icons.net/faq.html)はCC BYと各作者のクレジットを案内。SonnissのGDCライセンスは原則ゲーム組込み可だがAI学習・単体販売を禁止。

### 見落としやすい重要事項

- **Mixkitの無料「Stock Music」はゲーム利用不可**。同じサイトの「Sound Effects Free License」と混同しないこと：[公式Music License](https://mixkit.co/license/modal/musicFree/)。
- Kenneyの**個別素材パックは無料でCC0**。一方 [Kenney Game Assets All-in-1](https://kenney.itch.io/kenney-game-assets) は有料の一括ダウンロード製品であり「すべてを無料で入手できる一括ZIP」ではありません。
- OpenGameArt、Freesound、itch.ioは**サイト全体の一律ライセンスがない**。素材単位で確認すること。
- Spotify / Apple Music / YouTubeとのAPI連携権限は、ゲーム用BGM・録音物の**利用ライセンスではない**。ゲームに音源を組み込む権利は別途必要。
- 「無料」「Royalty Free」「Commercial Use」は、**無条件の再配布権、音源配信権、素材集としての転売権を保証しない**。

## 2. シーン別の初期候補（採用決定ではない）

| 2Dゲーム | アイコン・UI | 2D VFX | SFX / ジングル | 備考 |
| --- | --- | --- | --- | --- |
| パズル・カジュアル | Kenney UI Pack / Pixel UI | Kenney Particle Pack | Kenney UI Audio / Interface Sounds | **CC0中心**で構成しやすい |
| ファンタジーRPG・カード | Game-icons.net / Kenney Pixel UI | Kenney Particle / Smoke | Kenney Impact / Music Jingles | Game-iconsは著作者クレジット必須 |
| シューティング・アクション | Game-icons.net / Kenney UI | Kenney Smoke / Particle | Kenney Impact / Sci-fi / Sonniss | 音声多重再生・強い点滅・負荷を確認 |
| 雰囲気重視の2D作品 | Kenney UI / 個別itch.io素材 | 個別itch.io / OpenGameArt素材 | Freesound / OpenGameArtの個別音源 | ライセンスが混在するため慎重に管理 |

**選定原則**：一作品内の描画スケール・線幅・色調・ピクセル比・音量・音色を統一。ブランドUIの既存[UIアセット規約](../design/ui-assets.md)と衝突させない。CC0素材から初期プロトタイプを構成し、CC BY / 独自ライセンス素材は必要な場合にだけ導入。

## 3. 必須の関連ドキュメント

- [ライセンス判断とクレジット記録](licensing.md)
- [採用する個別ファイルの台帳](asset-register.template.md)
- [ゲームメニューのCredits雛形](credits.template.md)
- [Webゲームの描画/音声/Cloudflare実装規約](web-implementation.md)
- [GAME-01〜GAME-24 公開前レビュー](../workflows/game-asset-review.md)
- [game-assets Agent Skill](../../.agents/skills/game-assets/SKILL.md)

**採用範囲**：このリポジトリにあるのは候補の資料とワークフローのみ。**上記の素材そのものは追加・コピーしていません。** 実際に使うファイルを決めた案件では採用時点の公式ライセンスの保存と検証が必要です。
