# 2D Webゲーム素材の選定・ライセンス・品質レビュー（GAME-01〜GAME-24）

調査: 2026-11-17 / [候補一覧](../game-assets/README.md) / [許諾](../game-assets/licensing.md) / [台帳](../game-assets/asset-register.template.md) / [Web実装](../game-assets/web-implementation.md)

**この表は利用可能素材の選定手順です。素材の権利取得や実アプリでのテストが完了したことを意味しません。**
判定: `Pass / Fail / Not tested / Not applicable / Needs permission`。重要事項を`Not tested`のまま公開承認しない。

## ライセンス・出典（GAME-01〜GAME-10）

| ID | 公開前チェック | 想定する証拠 | 初期状態 |
| --- | --- | --- | --- |
| GAME-01 | 実際に採用したアイコン/SFX/VFX/BGMを**ファイル単位**で台帳登録した | Asset register /ファイルリスト | Not tested |
| GAME-02 | 素材固有の**公式URL、作者、版/ダウンロード日**が明らか | 原本ページ・ライセンス控え | Not tested |
| GAME-03 | CC0 / CC BY / 独自ライセンスを区別し、ライセンス全文と許諾条件を確認 | URL、保存した条件 | Not tested |
| GAME-04 | 商用・無料配布・広告/課金・改変を行う場合の権利を確認した | 該当ライセンス/許諾 | Not tested |
| GAME-05 | CC BY等の著作者/出典/ライセンス/変更表示がゲーム内Creditsにある | 画面・リンク・作者表 | Not tested |
| GAME-06 | Mixkit SFXと**ゲーム利用不可のMixkit Stock Music**を混同していない | 個別ファイルと規約 | Not tested |
| GAME-07 | FreesoundのNC・用途不明・権利侵害疑いの素材を使っていない | 採用表・追加許可控え | Not tested |
| GAME-08 | OpenGameArt/itch.ioは**投稿/パックごと**のライセンスと互換性を確認 | 投稿ページ・配布文書 | Not tested |
| GAME-09 | SonnissのAI学習禁止、単体販売禁止、独自ライセンスを評価した | GDC License、利用形態 | Not tested |
| GAME-10 | GitHub・R2・完成ゲームでの**素材生データの配信/再配布**が許諾範囲にある | 配信URL、契約、責任者承認 | Not tested |

## UI/音声/描画・操作（GAME-11〜GAME-18）

| ID | 公開前チェック | 想定する証拠 | 初期状態 |
| --- | --- | --- | --- |
| GAME-11 | UIアイコンが線幅・サイズ・色・ピクセルスケールに一貫性を持つ | 画面/スプライトシート | Not tested |
| GAME-12 | アイコン・音声・視覚エフェクトだけで重要な状態を伝えていない | UI/非音声フィードバック | Not tested |
| GAME-13 | Web Audio/HTML audioが**ユーザー操作後**に許可され、再生ブロックも扱う | モバイル/PCで再生確認 | Not tested |
| GAME-14 | SFX/BGMミュート・音量制御があり、音声の重なり/音割れを防ぐ | 実プレイ・メータリング | Not tested |
| GAME-15 | Safari/Chrome/Firefox/モバイル端末で音声codecと読み込みを確認 | 端末/ブラウザ試験 | Not tested |
| GAME-16 | 2D VFXがWCAG 2.3.1点滅しきい値を満たす | 実VFX動画/点滅レビュー | Not tested |
| GAME-17 | reduced-motionと必要に応じ画面揺れOFF/低刺激設定が働く | OS設定・ゲーム操作 | Not tested |
| GAME-18 | ピクセルアート描画倍率、テクスチャ/アトラス、FPS/メモリを検証 | モバイル/PC実測 | Not tested |

## 配信・保守・リリース（GAME-19〜GAME-24）

| ID | 公開前チェック | 想定する証拠 | 初期状態 |
| --- | --- | --- | --- |
| GAME-19 | SVGのスクリプト/外部参照等を精査し安全に読み込んでいる | SVG/DOMコードレビュー | Not tested |
| GAME-20 | R2/静的配信のCORS、MIME、キャッシュ、ライセンス配信条件を検査 | ブラウザNetwork・バケット設定 | Not tested |
| GAME-21 | 音声・画像の初回ロード予算/遅延ロード/失敗時の代替がある | Network/Lighthouse/実プレイ | Not tested |
| GAME-22 | 外部音楽APIの契約をゲームBGMの利用許諾と誤解していない | API連携仕様・音源台帳 | Not tested |
| GAME-23 | ゲーム利用者がCreditsに到達でき、掲載/リンクが正しい | Webメニュー・リンク試験 | Not tested |
| GAME-24 | **GAME-01〜23**の結果・ライセンス・未解決リスクに承認者と対応期限がある | 結果表、所有者、リリース記録 | Not tested |

## レビュー手順

1. [2D素材候補](../game-assets/README.md)から用途別の候補を選ぶ。**権利的に簡潔なKenney CC0を初期案として優先**。
2. 実際のファイルを選び、[素材台帳](../game-assets/asset-register.template.md)へ作者/ライセンス/元URL/版/変更/配信先を記録。
3. 条件が不明なら**保留・差し替え・権利者に確認**。独自/NC/ND/SA/GPLなどを一律に許可しない。
4. 必要なクレジットを[Creditsテンプレート](../game-assets/credits.template.md)でゲーム中から参照可能にする。
5. React/Vinext/Cloudflareの[実装方針](../game-assets/web-implementation.md)でaudio unlock・VFX刺激・CORS・容量・安全性を検査。
6. 24項目の判定と、Q/SEC/Privacyに該当する検査を統合。未確認を合格にしない。

## 例外・公開判定

**原則公開不可:** 禁止された利用（Mixkit Stock Musicのゲーム組込み、商用環境でのNC、権利不明）、CC BYの著作者表示欠落、独自ライセンスのソース再配布禁止に抵触、強い点滅の未修正、使用権限を証明できない素材。

例外が合法的な追加許可によるものなら、**許諾主体、原文、日付、適用範囲、責任者**を記録。重大な未確認項目のまま承認しない。

**リリースレポートの記録様式**

```text
Game / Commit: 【要記入】
Asset register: 【要記入】
Source/version/license checked: 【要記入】
GAME-01..24: 【Pass/Fail/Not tested/N/A/Needs permission】
Credits URL: 【要記入】
Mobile audio / VFX testing: 【要記入】
R2/static/source distribution: 【要記入】
Outstanding issues & owner: 【要記入】
Release decision: 【Go/No-Go/Pending】
```
