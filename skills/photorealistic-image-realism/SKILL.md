---
name: photorealistic-image-realism
description: 参考画像から「実在の人物を撮影した写真」に見える要素を分解し、再利用可能な実写化・フォトリアル生成プロンプトへ変換するスキル。肌、髪、目、唇、手、衣装、素材、照明、被写界深度、レンズ、構図、物理的接触、微細な不完全性を個別評価し、モデル非依存のプロンプトと必要に応じたQwen Image 2.1向け調整を作る。
license: MIT
argument-hint: "[analyze|prompt|adapt] [quick|full] [参考画像・生成条件・対象キャラクター]"
---

# photorealistic-image-realism

参考画像を「高精細な絵」ではなく「実際のカメラで撮影された人物写真」として分析し、フォトリアル生成に再利用できる形へ変換する。

## 目的

このスキルの目的は、単に photorealistic を追加することではない。

参考画像に含まれる以下の要素を分離し、どの要素が写真らしさに寄与しているかを判断する。

- 人体・顔の自然さ
- 肌の微細な質感と不均一性
- 髪の毛流れ、毛束、浮き毛
- 眼球、角膜、虹彩、まつ毛
- 唇や皮膚の湿度感
- 手・指・関節
- 衣服と身体の接触
- 素材ごとの反射特性
- 金属・革・布などの物理的質感
- 光源と環境光
- 被写界深度
- レンズ由来の表現
- 構図と撮影距離
- 背景との距離感
- 微細なランダム性・非対称性
- 「完璧すぎない」写真らしさ

最終的には、これらを「人物」「肌」「髪」「目」「衣装・素材」「照明」「カメラ」「構図」「全体表現」「避ける要素」に整理して提示する。

## 基本原則

### 1. 高精細さと実写感を混同しない

実写感は解像度だけでは生まれない。

特に重視するのは、

高精細 + 微細な不均一性 + 物理的な光 + 素材感 + 空間的な奥行き

の組み合わせ。

「超高精細」「8K」などの品質語だけで説明を終わらせない。

### 2. 完璧さを減らす

フォトリアル生成では、過度な左右対称、完全に均一な肌、完全に整列した髪、プラスチックのような皮膚などが、かえってCG感を強める。

以下を適度に残す。

- subtle facial asymmetry
- natural skin variation
- fine stray hairs
- irregular hair strands
- subtle skin texture
- slight material imperfections
- natural finger and joint variation

ただし、破綻や奇形を意図的に増やすこととは区別する。

### 3. 「キャラクター絵」ではなく「撮影された人物」として考える

構図を評価するときは、

「このキャラクターをどう描くか」

ではなく、

「この人物をカメラマンがどう撮影したか」

という視点を優先する。

特に有効な要素：

- portrait photography
- live-action appearance
- realistic photographic exposure
- shallow depth of field
- natural bokeh
- professional portrait lens
- realistic lens rendering
- physically consistent lighting

### 4. 物体同士の関係を評価する

実写感は個々のパーツだけでなく、物体同士の接触から生まれる。

例：

- 手が眼帯に触れる
- ベルトが衣服を押さえる
- 金具が布を固定する
- 髪が肩や衣服に乗る
- 布が身体の曲面に沿って変形する

「ポーズ」ではなく、物理的な接触・圧力・変形まで見る。

## ワークフロー

### Step 1: 撮影としての全体像を判定

最初に以下を短く判定する。

- 写真 / 3DCG / イラスト / 混合
- ポートレート / 全身 / スナップ / コスプレ撮影
- カメラとの距離
- 主光源の方向
- 背景の距離
- 被写界深度の強さ
- 全体の色温度

### Step 2: 顔・人体を分解

次を個別に確認する。

#### 顔

- 骨格
- 左右非対称性
- 頬・鼻・額の色変化
- 目の周辺の陰影
- 唇の境界
- 肌の微細な質感

#### 目

- 虹彩
- 瞳孔
- 角膜
- キャッチライト
- 白目の陰影
- まぶたの影
- まつ毛

#### 手

- 指の長さ・太さ
- 関節
- 指先
- 爪
- 手袋やアクセサリーとの接触
- 物体との接触状態

### Step 3: 髪を分解

次を重視する。

- 根元から毛先までの毛流れ
- 大きな毛束と細い毛束の混在
- 浮き毛
- 顔周辺の細い毛
- 光を受けた毛束
- 影側の毛束
- 完全に整列していないランダム性

### Step 4: 衣装・素材を分解

素材ごとに光の反応を分ける。

#### 革

- smooth but non-plastic surface
- controlled specular highlights
- natural creases
- subtle surface variation

#### 布

- soft diffuse response
- fabric folds
- tension around body
- compression under belts or accessories

#### 金属

- edge highlights
- reflected environment
- dark reflected areas
- realistic thickness

素材名だけを書くのではなく、「光にどう反応するか」を書く。

### Step 5: 照明を分析

以下を抽出する。

- key light
- fill light
- rim light
- ambient light
- color temperature
- shadow softness
- reflected environmental color

人物・衣装・背景が同じ空間の光を共有しているかを確認する。

### Step 6: カメラ表現を分析

以下を推定する。

- portrait lens impression
- focal distance
- shallow/deep depth of field
- focus plane
- background bokeh
- optical softness
- edge sharpness
- exposure
- contrast
- subtle optical imperfections

「レンズ名」を断定する必要はない。見た目から判断できる範囲で表現する。

### Step 7: 実写感を壊す要素を抽出

次のような要素がある場合はnegative prompt候補にする。

- plastic skin
- porcelain skin
- perfectly symmetrical face
- overly smooth skin
- uniform hair strands
- painted eyes
- flat lighting
- CGI materials
- excessive beauty retouching
- unnaturally sharp details everywhere
- perfectly clean edges
- flat black clothing

### Step 8: プロンプトへ変換

最終出力は次の構造を基本とする。

1. Core photorealism
2. Face & anatomy
3. Skin
4. Hair
5. Eyes
6. Hands
7. Clothing & materials
8. Lighting
9. Camera & depth
10. Composition
11. Natural imperfections
12. Negative prompt
13. Model-specific notes

同じ意味の形容詞を大量に重ねない。各語句には役割を持たせる。

## 出力ルール

### analyze

参考画像を分析するときは、単なる感想ではなく「観察 → 写真らしさへの寄与 → 再利用語句」の順で整理する。

例：

- 観察：顔の左右が完全には対称ではない
- 寄与：人工的な3Dモデル感を減らす
- 再利用：subtle facial asymmetry

### prompt

再利用可能なプロンプトを作る。

「この画像そっくりにする」ことではなく、画像から抽出した一般化可能な撮影・質感要素を使う。

### adapt

既存のキャラクターや衣装を維持したまま、写真らしさだけを強化する。

キャラクター固有設定を勝手に変更しない。

## モデル別調整

### Qwen Image 2.1

Qwen Image 2.1で使う場合は、品質語を過剰に増やすより、具体的な視覚情報を優先する。

優先順位：

1. 人物・衣装の具体性
2. 光源
3. 素材
4. カメラ・被写界深度
5. 肌・髪・目の微細な情報
6. 最後に品質語

写実化だけを目的とする場合、photorealistic だけでなく、live-action appearance、realistic photographic exposure、natural skin texture、physically consistent lighting など役割の異なる表現を組み合わせる。

## 重要な区別

以下を混同しない。

- 「美しい」≠「実写」
- 「高精細」≠「実写」
- 「リアルな色」≠「実写」
- 「毛穴を大量に描く」≠「実写」
- 「ノイズを加える」≠「写真」
- 「ぼかす」≠「被写界深度」

写真らしさは、人体・素材・光・レンズ・空間の一貫性から生まれる。

## 安全性・人物同一性

参考画像から人物の実在性や本人性を推定しない。

「実在の人物に見える」という評価は、画像の視覚的特徴についてのみ述べる。

実在人物の顔を別人物へ移植する依頼など、本人同一性が重要になる場合は、必要に応じて安全性を確認する。
