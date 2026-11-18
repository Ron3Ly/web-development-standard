# 2D Webゲーム素材の実装標準（React / Vinext / Cloudflare）

調査: 2026-11-17。対象: ブラウザ2Dゲーム。関連: [素材候補](README.md) / [ライセンス](licensing.md) / [GAMEレビュー](../workflows/game-asset-review.md) / [品質標準](../quality/quality-standard.md)。

## 1. フロントエンドと2D描画

- React/Vinextはメニュー、設定、成績、認証などのアプリUIに利用。Canvas/WebGLのゲームループやAudioContextは**Client Component / ブラウザ側**で扱い、RSCサーバーコードに`window`や`AudioContext`を直接置かない。
- 連続する2D VFXのフレームを**React stateの高頻度更新で描画し続けない**。必要に応じゲームループの`requestAnimationFrame`、Canvasや既存2Dエンジンの標準APIを使用。
- ピクセルアートは必要に応じ`image-rendering: pixelated`や整数倍率のスケールを検討。高DPR/非整数比では見え方が変わるため、iOS/Android/PCで検証。
- スプライトシート/テクスチャアトラス、読み込み画面、必要なタイミングでのデコード/キャッシュで初回転送・VRAM負荷を抑える。画像/アイコンSVGは信頼できるファイルを選び、外部リンク/Scriptを含む未検証SVGのインライン注入は避ける。
- ゲームアセットに用いるUIのタップターゲット、視覚効果、色の区別は既存の[デザイン標準](../design/design-system.md)およびアクセシビリティ指針に従う。

参考: https://developer.mozilla.org/en-US/docs/Games/Techniques/Crisp_pixel_art_look 、https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Animation_performance_and_frame_rate

## 2. 音声（SFX / BGM）のブラウザ制限

- **ページ表示時の可聴自動再生を前提にしない**。最初のユーザー操作（「ゲーム開始」「音を有効化」など）によりAudioContextを作成/再開する。再生拒否もUIで扱う。
- SFX（短い効果音）とBGM/環境音は音量を**別々に制御**し、必ずミュート設定を用意。初回から大音量にしない。設定は必要に応じて利用者が保存できるようにする。
- ジャンプ、攻撃、報酬取得、失敗、アラートなど**重要な状態は音だけで伝えない**。文字/視覚/その他の非音声フィードバックを提供。必要に応じて字幕・視覚合図を設ける。
- 高速な打撃・連打時、SFXの音声が大量に重なって音割れや高CPUにならないよう同時ボイス数・音量・クールダウン等を調整。
- `HTMLAudioElement` と Web Audio API は用途別に選択。短い音は事前ロード/デコードを検討、長いBGMはストリーミングを検討。メモリ使用量を確認。
- OGG/MP3/WAV等はブラウザ・端末・デコーダでの実動作を確認。**すべてのブラウザが全codecを同じようにサポートするとは仮定しない**。必要なら許可された範囲でトランスコードする。
- タブがバックグラウンドに移った場合の停止/一時停止/復帰、オーディオコンテキスト再開、モバイルのオーディオセッション切替に対応。

一次資料: https://developer.mozilla.org/en-US/docs/Games/Techniques/Audio_for_Web_Games 、https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Best_practices 、https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay

## 3. パーティクル・爆発・点滅のアクセシビリティ

- **WCAG 2.2の2.3.1（Level A）**「1秒間に3回を超える点滅、または一般閾値/赤色閾値を超える点滅」を避ける。単に設定で消せるだけでは、起動直後の発作リスクを消せない。
- `prefers-reduced-motion: reduce` に応じて、不要な大きな移動・画面揺れ・ズーム・持続的パーティクルを減らす/停止する。ゲーム内の「画面揺れOFF」「低刺激」も検討。
- VFXはヒット/成功/重要状態を補助するが、**色・音・点滅だけに依存しない**。視認性が低い状態でも結果が伝わるテキスト/ゲージ/アイコンを残す。
- 端末性能に応じてエフェクト粒子数・透明ブレンド・テクスチャサイズを制限。FPS/メモリ/入力応答性を実端末で確認。
- 強い光や激しい点滅のアセットは採用前に実際のアニメーションを検証し、しきい値や代替表示を満たさなければ使用しない。

参考: https://www.w3.org/WAI/WCAG22/Understanding/three-flashes-or-below-threshold 、https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/%40media/prefers-reduced-motion

## 4. Cloudflare Workers / R2で配信する場合

- Webゲーム素材は静的アセット/アプリの公開物として提供するか、実際に必要なときにR2から配信する。**R2採用は必須ではない**。
- 著作権・利用許諾の判断が先。MixkitやSonnissのような独自契約の**ソース/単体再配布に制限のある素材**を、公開R2直リンクや公開GitHubで配ってよいとは自動判定しない。
- R2の配信URL、CORS、Cache-Control、ファイル容量とContent-Type、公開範囲、期限を確認。ユーザー別に権限のある画像/素材は[SECのR2制御](../security/cloudflare-hardening.md)に従う。
- 大量の音声と高解像度エフェクトの初期一括ロードを避け、ゲーム進行に必要なものからロード。キャッシュ無効化用のバージョン/ハッシュを維持。
- サードパーティ素材を公開するときは[素材台帳](asset-register.template.md)に配信方式、ハッシュ/元バージョン、クレジット、採用承認を記録。
- Spotify / Apple Music / YouTube APIの認証・検索機能はゲーム音源の**再利用権ではない**。ストリーミング音楽の無断取得や再配信を行わない。

## 5. 最小の実装チェック

- [ ] ゲームの開始時に音声が許可されなくてもプレイ開始できる
- [ ] ミュート・音量設定と復帰/非音声通知がある
- [ ] Safari・Chrome・Firefox・iOS/Androidで採用フォーマットを実際に確認
- [ ] SVG/画像の安全性と、スプライト/パーティクルの描画負荷を確認
- [ ] 2.3.1点滅のしきい値とreduced-motionを確認
- [ ] Assetsの公開配布形態、CORS/Cache/ライセンスを確認
- [ ] 著作者表示が必要な素材のCredits画面に到達できる
- [ ] 計測不能な項目は`Not tested`と明示

**ゲームエンジン（Phaser/PixiJS等）の追加は本規約の必須ではない。** プロジェクトに採用済みの技術スタックとライブラリを尊重する。
