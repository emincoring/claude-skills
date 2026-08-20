---
name: seedance-2-5-prompt
description: ByteDance/Dreamina「Seedance 2.5」向けの動画生成プロンプトを作成・改善するためのSkill。ユーザーが「Seedance」「Seedance 2.5」の動画生成プロンプトを書いて/作って/生成して/直してほしいと依頼した場合、または「AI動画のプロンプトを書いて」など文脈からSeedance用途と分かる場合は、日本語・英語の依頼を問わず必ずこのSkillを使用すること。プロンプトの構成要素、カメラワーク用語、ライティング/スタイル語彙、音声（セリフ・効果音・BGM）の指定方法、ネガティブプロンプト、推奨設定（解像度・アスペクト比・尺・マルチモーダル参照）を含む。
tags: [seedance, dreamina, capcut, video-generation, prompt-engineering, ai-video]
---

# Seedance 2.5 プロンプト生成Skill

ByteDance / CapCut Dreaminaの動画生成モデル「Seedance 2.5」向けに、高品質なプロンプトを作成・改善するためのSkillです。

> **出典についての注記**: 本Skillはユーザー指定の以下3URLを参照する意図で作成しましたが、このセッションのネットワークegressポリシーにより `dreamina.capcut.com` と `bytedance.larkoffice.com` への直接アクセスがブロックされていたため、WebSearch経由で得られる同内容の引用・要約情報（Dreamina公式ページのキャッシュ/引用を含む複数の二次情報源）を基に再構成しています。
> - https://dreamina.capcut.com/ja-jp/seedance/seedance-2-5-prompt
> - https://dreamina.capcut.com/ja-jp/seedance/seedance-2-5-best-settings
> - https://bytedance.larkoffice.com/wiki/NjnWwvf4BiFYFLk2RzrcEgaunGf （ByteDance社内Larkドキュメントのため、外部からは原理的にアクセス不可の可能性が高い）
>
> ユーザーが上記ページの本文を直接貼り付けてくれた場合は、その内容でこのファイルを更新・上書きすること。

---

## 1. Seedance 2.5 の概要

- ByteDance製の最新動画生成モデル。CapCut Dreaminaで利用可能。
- 1回の生成で最大30秒のワンショットを高い一貫性で生成でき、「拡張モード（Extended Mode）」では5秒〜180秒のマルチシーン動画も生成可能。
- 音声・映像を同一の潜在空間で同時生成（後付け合成ではない）。セリフ・効果音・環境音・BGMをプロンプトで直接指定できる。
- 最大50個のマルチモーダル参照（画像・動画・音声・3Dアセット等）を同時投入し、キャラクターやシーンの一貫性を保てる「Omni-Modal Reference Mode」を搭載。
- 特定領域だけを再生成する「ローカル/リージョン編集」に対応（例: 背景や商品の色だけ変更し、他はそのまま保持）。
- 4K・10bitカラーでの出力に対応（画質設定は下記「推奨設定」参照）。

---

## 2. 基本プロンプト公式

```
Subject（被写体） + Action（動作・一連の動き） + Camera（画角・カメラワーク） + Lighting（光） + Style（スタイル） + (Audio（音声）)
```

- 1〜3文程度に収める。長い曖昧な文章より、短く具体的な文章の方が良い結果になる。
- **1つの連続したアクション**だけを描写する。無関係な動作を複数詰め込むと、被写体が崩れたり動きがカクついたりする原因になる。
- 「cinematic」のような曖昧な形容詞だけに頼らず、具体的なカメラ用語・光の種類を書く方が精度が上がる。

### 各要素の内訳

| 要素 | 内容 |
|---|---|
| Subject | 誰が/何が、どんな状態・服装・特徴か |
| Action | 何をしているか（1つの連続した動作） |
| Camera | ショットサイズ、アングル、カメラの動き、フォーカス対象、レンズ/カメラ機種 |
| Lighting | 光源の種類、色温度、コントラスト、時間帯 |
| Style | 映像のルック（実写/アニメ/フィルム調など）、色調、質感、時代感 |
| Audio（任意） | セリフ、効果音、環境音、BGMのムード |

### プロンプト例（英語で記述するのが基本。日本語で依頼された場合も、実際に投入するプロンプト本文は英語で書き、日本語で補足説明を添えるとよい）

```
A young barista in a green apron steaming milk behind a marble counter, turning to smile at the camera.
Slow dolly-in from a wide shot to a medium close-up.
Warm morning light through a side window, soft shadows.
Cinematic, shallow depth of field, 35mm film look.
Ambient café noise and the hiss of the steam wand.
```

---

## 3. カメラワーク語彙（Camera）

**構造化テンプレート**: `Camera: [動きの種類] + [速度] + [対象ロック]` に加えて `Stability`（tripod/handheld/gimbal）と任意で `Lens hint` を添える。

### ショットサイズ
- Extreme close-up（超クローズアップ）
- Close-up（クローズアップ）
- Medium shot / Medium two-shot（ミディアムショット）
- Full shot / Wide establishing shot（フルショット/確立ショット）
- Over-the-shoulder（オーバーザショルダー）

### カメラの動き
- 被写体に寄る: slow push-in / dolly-in / zoom toward
- 引く: pull back / dolly-out
- 横移動: pan left/right（カメラの首振り）、track left/right（カメラ本体が並走）
- 周回: orbit left/right、360-degree orbit（商品撮影に有効）
- 上下: tilt up/down、crane up/down
- 固定: locked-off static shot
- 手持ち風: subtle handheld drift、documentary-style handheld camera

### 速度・トーンの修飾語
`very slow` / `gradual` / `quick` / `sweeping` / `smooth` / `aggressive` / `handheld`

### レンズ・機材の指定(スタイルに強く影響する)
例: `ARRI Alexa Mini LF at 24fps, 35mm prime at T1.8` / `Sony Venice with vintage Cooke lenses` / `RED Komodo, anamorphic`
→ カメラ機種・レンズ・絞り（被写界深度に影響）を1文で指定すると、モデルの画作りの傾向が大きく変わる。

### ⚠️ 注意点
- **1クリップにつきカメラの動きは1〜2個まで**。パン・オービット・クレーンを同時に要求すると、映像が不安定になりやすい。
- **矛盾する指示を避ける**（例:「固定ショットなのに素早くオービットする」は指示が相殺し合う）。

---

## 4. ライティング語彙（Lighting）

光は映像の感情的なトーンを決める。具体的に書くほど4K/10bit出力にディテールが残る。

例:
- `golden hour backlight`（黄金時間の逆光）
- `soft overcast daylight`（曇天の柔らかい自然光）
- `moody neon from the left`（左からの陰鬱なネオン光）
- `warm morning light through a side window, soft shadows`

---

## 5. スタイル語彙（Style）

色・光・質感・時代感などの「画のルック」を決める要素。「正しい絵」から「映画的な絵」に引き上げる部分。

例: `cinematic, shallow depth of field, 35mm film look` / `healing fresh` / `aesthetic literary` / `Japanese fresh` / `Korean atmosphere` / アニメ調・実写調などテーマに応じて指定。

---

## 6. 音声プロンプト（Audio）

Seedance 2.5は映像と音声を同時生成するため、音の指定も画作りと同じ重みで扱う。

**4レーンで計画する**: セリフ（dialogue）／物理効果音（physical effects）／環境音（ambience）／音楽（music）。それぞれ、目に見えるアクションやタイミングと紐づけるとよい。

- **セリフ**: 実際に発話させたい文言をそのまま引用符で囲んで書く。短いセリフの方がリップシンク精度が高い。
  例: `spoken dialogue: "Let's get started."`
- **音楽ムード**: `soft synth pad` / `building cinematic score` / `upbeat pop bed` のように簡潔に。
- **環境音+効果音は1つずつ**に絞る。詰め込みすぎるとミックスが濁る。

---

## 7. ネガティブプロンプト

不要なテキスト・余計な人物・意図しない音楽・アーティファクト・ブリーフから外れた結果を避けるために指定する。
- 例: `no text, no subtitles, no watermark, no extra people, no distorted hands`
- 参照画像にテキストが写り込んでいると、生成結果にも意図せずテキストが出やすいので避ける。
- 出力後は必ず目視でアーティファクトを確認する。

---

## 8. よくある失敗（避けるべきパターン）

1. **無関係な動作を複数詰め込む** → カクつき・変形の原因。1つの連続したアーク（弧）を描写する。
2. **矛盾するカメラ指示**（例: 「静止ショット」+「素早いオービット」）→ 指示が相殺される。1つのカメラの振る舞いに絞る。
3. **image-to-videoで参照画像の内容を過剰に説明し直す** → 参照画像に既に写っている内容を繰り返すとモデルが混乱する。差分（動き・カメラ・光の変化）だけを書く。
4. **曖昧な形容詞のみに頼る**（"cinematic"だけ等）→ 具体的なカメラ/光/レンズ用語を併記する。

---

## 9. 推奨設定（Best Settings）

- **解像度**: 480p / 720p / 1080p（1080pは提供状況が変動する場合あり。用途に応じて選択）
- **アスペクト比**: `Fit` / `Original` / `9:16`（TikTok・Reels・Shorts向け） / `1:1`（Instagram） / `3:4` / `16:9`（YouTube・横型） / `4:3` / `21:9`
  - **先にアスペクト比を決めてからプロンプトを書く**のが良い順序。
    - 縦型（9:16等）: 被写体1体に絞って強く描写する。
    - 横型（16:9等）: 背景の指示も入れる（例: "minimal background movement", "soft depth", "clean wall"のようなアンカー）。
- **尺（Duration）**:
  - 標準: 1回のワンショット生成で最大30秒（キャラクター・光・動きの一貫性を保ったまま単一パスで生成）。
  - 拡張モード: 5秒〜180秒、複数シーンにまたがる長尺・マルチシーン構成が可能。
  - 一貫性を最優先する場合は短めのクリップ（3〜4秒程度）の方が安定しやすい。
- **参照アセット（Omni-Modal Reference）**: 画像・動画・音声・スクリプト・キャラクターシート・絵コンテなど最大50個まで同時投入し、キャラクターやシーンの一貫性を保てる。
- **ローカル編集**: 既存クリップの一部領域（商品の色、背景の一部等）だけを差し替え、他の要素（グローバルな光・キャラクターの特徴）は保持したいときに使う。

---

## 10. プロンプト生成時の実施手順（このSkillを使うときの動き方）

ユーザーからSeedance 2.5用のプロンプト作成・改善を依頼されたら、以下の順で対応する。

1. **要件のヒアリング/推定**: 被写体、シーン、目的（縦型SNS用か横型か）、尺、トーン（実写/アニメ等）、音声の要否を確認する。不明点があり、生成結果に大きく影響する場合はユーザーに確認する。曖昧でも文脈から妥当に推測できる場合は、仮定を明示した上で進めてよい。
2. **アスペクト比・尺を先に決める**（用途から逆算: 例 TikTok→9:16、YouTube→16:9）。
3. **公式フォーマット（Subject + Action + Camera + Lighting + Style + Audio）に沿って英語のプロンプト本文を作成する**。日本語での依頼であっても、投入用プロンプト本文は英語で書き、日本語の解説・意図説明を添える。
4. **カメラの動きは1〜2個まで**に絞り、矛盾がないか確認する。
5. **必要に応じてネガティブプロンプトを付記**する。
6. **音声が必要な場合は4レーン（セリフ/効果音/環境音/音楽）を簡潔に指定**する。
7. 最終的に、①完成プロンプト本文（英語）②各要素の意図（日本語の簡単な解説）③推奨設定（解像度・アスペクト比・尺）をセットでユーザーに提示する。
8. 複数パターンが有効な場面（バリエーション依頼など）では、2〜3案を出し、それぞれの違い（カメラワークやトーン）を一言で添える。
