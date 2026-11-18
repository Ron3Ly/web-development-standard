# 2Dゲーム素材の利用許諾・著作権・配布チェック

調査: 2026-11-17。関連: [候補一覧](README.md) / [素材台帳](asset-register.template.md) / [クレジット](credits.template.md) / [GAMEレビュー](../workflows/game-asset-review.md)。

## 1. 許諾の判定（サイト単位ではなく素材ファイル単位）

| 区分 | 例 | 通常の判断 | 実装前に確認する項目 |
| --- | --- | --- | --- |
| **A: CC0** | Kenney個別CC0素材、Freesound上のCC0作品 | 商用ゲームへの組込み・改変が原則可能。帰属表示は通常不要 | **該当ファイル**が本当にCC0か、第三者の商標/キャラクター/実在人物等を含まないか |
| **B: CC BY** | Game-icons CC BY 3.0、FreesoundのCC BY 4.0 | 商用組込みは可能だが、**作者/素材/ソース/ライセンス/改変の表示**等が必要 | 版・クレジット表示・配信方法・追加制限の有無 |
| **C: カスタム/EULA** | Mixkit Free SFX、Sonniss GDC Bundle、itch.ioの独自条件 | ゲーム組込みが明記されている場合に限り候補 | ソース配布/素材単体配信・加工・クレジット・AI学習・利用地域・アカウント帰属 |
| **D: 個別確認** | OpenGameArtのCC BY-SA、GPL/OGA-BY、作者不明、特殊素材 | 自動で採用しない。成果物の配布形態・DRM・改変・ライセンス継承義務を検討 | 法的/契約上の影響や権利を確認し、承認者を記録 |
| **E: デフォルト禁止** | CC BY-NC商用ゲーム利用、ゲーム利用禁止音楽、条件不明、盗用疑い | **採用しない**。別許可を得た場合のみ再評価 | 独立した追加許諾があれば原文を保存 |

- **CC0**: https://creativecommons.org/publicdomain/zero/1.0/
- **CC BY 3.0**: https://creativecommons.org/licenses/by/3.0/
- **CC BY 4.0**: https://creativecommons.org/licenses/by/4.0/
- **CC BY-NC 4.0**: https://creativecommons.org/licenses/by-nc/4.0/
- **Creative Commonsの基本条件**: https://creativecommons.org/chooser/

ライセンスの「素材を利用できる」と「素材の生データを配布してよい」は**別の判断**。Webゲームではブラウザが画像/音声をダウンロードして表示するため、R2の公開URLや公開GitHubに裸の素材ファイルを置くことが**独自ライセンスのソース再配布禁止**に触れるか、契約と配信方式を確認する。

## 2. 提供元別の重要な条件

### Game-icons.net
- 原則 **CC BY 3.0**（一部CC0/Public Domain例外あり）。アイコンごとに**作者**を調べる。
- ゲームメニューの「Credits / 素材提供」ページを設け、作者・素材URL・ライセンス・変更があればその旨を記載。
- ロゴ、商標、実在キャラ等は素材のCC BY表示だけで別の権利が解決するとは限らない。
- 公式: https://game-icons.net/about.html / https://game-icons.net/faq.html

### Kenney
- Kenneyの公式資産ページのゲーム素材は**CC0**、帰属表記不要。個別パックのライセンスも記録する。
- 公式ロゴや「Kenney公式ゲーム」を示すブランド表現まで自由になるとは扱わない。
- All-in-1バンドルは**有料の利便商品**。個別パックのCC0ライセンスと販売価格は別。
- 公式: https://kenney.nl/support

### OpenGameArt / Freesound / itch.io
- **作品ページの個別ライセンス**が最重要。作家によるクレジット指定を尊重。
- OpenGameArtにはCC0、CC BY、CC BY-SA、OGA-BY、GPL等がある。シェアアライク・GPLは二次創作の配布条件の検討が必要。自動で「クローズド商用ゲームに問題なし」と判定しない。
- FreesoundはCC0/CC BY 4.0/CC BY-NC 4.0など。NCは**商用ゲームに使わない**（別の許可が必要）。
- itch.ioの「Commercial Use」タグがあっても、クレジット、単体再配布禁止、改変や商標の制限が異なる。
- 公式: https://opengameart.org/content/faq / https://freesound.org/help/faq/ / https://itch.io/game-assets/tag-commercial-use

### Mixkit
- **Sound Effects Free License**: 映像/ゲームの完成品への組込みは許可。素材を単体・素材集・テンプレート・ソースファイルとして再配布することは禁止。
- **Stock Music Free License**: **Video Gamesは「Not Allowed」**。効果音の利用許可と混同しない。
- ゲームのリポジトリ、R2の直リンクなどで生素材を配布する場合は、完成品の一部としての配信と禁止された素材配布を慎重に区別。不明なら採用を保留。
- 公式: https://mixkit.co/license/modal/sfxFree/ / https://mixkit.co/license/modal/musicFree/

### Sonniss GDC
- GDC Bundleの**契約条件**でゲーム組込み・商用・改変が許可され、著作者表示は不要。
- 音声そのものの販売や「自作録音」の虚偽表示は禁止。**AI/MLの開発・学習目的への利用は不可**。
- AI開発パイプラインへの素材投入がその禁止用途に該当するか不明な場合は利用前に確認。モデルに音声を無断提供しない。
- 容量が大きい元の音声を標準リポジトリへ再配布せず、必要な部分を案件側で確認して採用。
- 公式: https://sonniss.com/gdc-bundle-license/

## 3. クレジット表記と保存

**CC BY等でクレジットが必要な場合：** 通常は「タイトル、作者、出典URL、ライセンス名＋URL、改変内容」を記録し、ゲーム内のメニューやフッターから到達可能なCreditsページへ。原作の制作者がゲームを推薦しているように表現しない。

**例（実際の作者・作品に置き換えるまで公開不可）**

```text
[素材タイトル] — [作者名]
出典: https://[素材固有URL]
ライセンス: CC BY 3.0 (https://creativecommons.org/licenses/by/3.0/)
変更: [色変更/リサイズ/変更なし]
```

CC0でも採用元を台帳に残す。ライセンスの保存時点（チェック日）、作者名、ダウンロード版/ファイル名、ハッシュ、実際のクレジットページ、ライセンスのスクリーンショットまたは保存したライセンス文章（条件を守った範囲）を案件で保管する。

## 4. 再配布・GitHub・R2の禁止事項

- **この共通リポジトリは素材カタログです。** 権利のない第三者ZIP/音声・ゲームアセット全体をcommitしない。
- **公開GitHub**にカスタムライセンス素材の元ファイルを入れない。Mixkit/Sonnissなどは特に完了作品に限定された利用権の範囲を確認。
- **Cloudflare R2**に置く場合、バケット公開範囲、CORS、Cache-Control、直リンク、署名URL等の運用を確認。署名URLやロックだけでライセンス問題が解決するとは限らない。
- 利用素材が転載・盗用の疑い、著作権を侵害するキャラクター/音声、追加の商標/パブリシティ権等を含む場合、ライセンス表記があるだけで採用しない。
- 実際の配布可能性に疑いが残る素材は**未承認**扱い。別のCC0/CC BY代替を探す。

## 5. 公開前の判定
[GAME-01〜GAME-24](../workflows/game-asset-review.md) のうち、ライセンス・作者・帰属表示・公開方法のチェックを必須化。**条件不明・NC・使用禁止素材・クレジット漏れは原則リリースブロッカー**。権利に疑いがある場合は専門家へ確認し、未確認をPassにしない。

この文書はライセンスを要約した**実装用のチェック基準**であり、個別契約に対する法律意見ではありません。
