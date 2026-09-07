# ComfyUI Native Pixal3D & TRELLIS.2 Tuning Workflow

[English](README.md)

ComfyUI公式の **Pixal3D & TRELLIS.2: Image to Model** テンプレートをベースに、両モデルの比較とパラメータ調整をしやすく再構成した実験用ワークフローです。

新しいモデルや独自推論ノードではありません。公式ネイティブパイプラインを維持しながら、CFG・Steps・Seed・FOV・Shape Upscale・Remesh・Texture Bake・GLB保存などを操作面へ露出した派生版です。

> 収録値は万能な最適値ではなく、比較・検証を始めるための実験的プリセットです。

![ワークフロー全体](docs/images/workflow-overview.webp)

<details>
<summary>調整パネルの拡大画像</summary>

![共通・Pixal3D・TRELLIS.2調整パネル](docs/images/tuning-controls.webp)

</details>

## このワークフローの目的

公式テンプレートは一連の3D生成を網羅していますが、反復調整する値はグラフ全体に分散しています。この派生版では、調整値を次の3領域へ集約しています。

- **Pixal3D Custom Parameters**
- **TRELLIS.2 Custom Parameters**
- **Custom Parameters（共通設定）**

両モデルに別々の値を保持したまま、Model Booleanだけで比較できます。

## 公式テンプレートとの差分

以下はスクリーンショットからの推測ではなく、提供された公式版`3d_pixal3d_trellis2_image_to_model` JSONとの機械比較結果です。

| Graph | Nodes | Groups | Links |
| --- | ---: | ---: | ---: |
| 公式テンプレート | 66 | 12 | 100 |
| 本ワークフロー | 121 | 15 | 169 |

追加・変更したもの：

- 共通／Pixal3D／TRELLIS.2の3つの操作グループ
- Structure／Shape／Upsample／TextureのCFG・Stepsをモデル別に切り替える配線
- Structure／Shape／Texture Seedの中央集約
- `0.0`で公式のMoGe推定経路へ戻る手動FOV Override
- Texture StageとShape Decodeの両方へ反映されるShape Upscale迂回
- モデル別Remesh Smooth Iterations／Project Back
- Target Face Count／Decimate Placement Modeの中央操作
- Normal Map Cage Distance `0.02`の外部入力化
- Sparse Mesh、Decoded Shape、Painted Decoded Shape、Post-processed Mesh、Vertex-color Meshの5地点に`Render Mesh → Preview Image`を追加
- 公式Advanced 3D Saveとは別に通常版`Save GLB`を追加

次の項目は公式から継承したもので、変更点としては扱いません。

- Pixal3D／TRELLIS.2の公式ネイティブ生成処理と元のモデル切替ロジック
- 背景除去、Crop、DINOv3 Conditioning、MoGe FOV推定
- Structure、Shape、Upsample、Texture、Remesh、Decimate、UV Unwrap、PBR Bake
- Texture Resolution 4096
- Base Color、Roughness、Metallic、Normal、AOのBake／Preview
- Vertex Color経路とAdvanced 3D Preview／Save

サードパーティ製の生成ノードパックは必要ありません。

## ファイル構成

- `workflows/native_pixal3d_trellis2_tuning.json`：配布ワークフロー
- `examples/input-reference.jpg`：調整時に使用した入力画像
- `docs/images/workflow-overview.webp`：ワークフロー全体図
- `docs/images/tuning-controls.webp`：操作部分の拡大図
- `CIVITAI.md`：Civitai掲載用の本文・タグ・投稿チェックリスト
- `CHANGELOG.md`：変更履歴

## 動作確認環境

| 項目 | バージョン／構成 |
| --- | --- |
| ComfyUI | 0.34.0 |
| ComfyUI Frontend | 1.51.10 |
| Workflow Templates | 0.11.55 |
| OS | Linux |
| GPU | NVIDIA GeForce RTX 5070 Ti、VRAM 16GB |
| Python | 3.13.14 |
| PyTorch | 2.14.0.dev20260723+cu132 |

JSON内のFrontendメタデータは`1.49.6`ですが、その後Frontend `1.51.10`環境で実行されています。上記以外の環境は未確認です。

2026年8月に追加されたPixal3D／TRELLIS.2公式ネイティブ対応を前提としています。ノード不足が出る場合はComfyUI本体とFrontendの両方を更新してください。

## 必要モデル

JSONは次のファイル名を参照しています。

```text
trellis_2_int8_convrot.safetensors
pixal3d_int8_convrot.safetensors
trellis_2_shape_vae_bf16.safetensors
trellis_2_texture_vae_bf16.safetensors
dino_v3_L_naf_fp32.safetensors
moge_2_vitl_normal_fp16.safetensors
birefnet.safetensors
```

配布URLと配置先は更新される可能性があるため、公式の案内を参照してください。

- 公式ワークフロー：https://comfy.org/workflows/920c795f693a-920c795f693a/
- 公式チュートリアル：https://docs.comfy.org/tutorials/3d/trellis2

## 使い方

1. ComfyUI本体とFrontendを更新する。
2. 公式テンプレートが要求するモデルを配置する。
3. `workflows/native_pixal3d_trellis2_tuning.json`をComfyUIへドラッグする。
4. `Load Image`で入力画像を選ぶ。
5. **Model**を`false`でPixal3D、`true`でTRELLIS.2にする。
6. 必要に応じてモデル別パラメータを変更する。
7. Queueしてレンダーと各Texture Mapを確認する。
8. GLBは`ComfyUI/output/3d/ComfyUI`以下へ保存される。

## 収録プリセット

品質順位や万能設定ではなく、作者が比較を始めるための実験値です。

| Stage | Pixal3D CFG | Pixal3D Steps | TRELLIS.2 CFG | TRELLIS.2 Steps |
| --- | ---: | ---: | ---: | ---: |
| Structure | 6.0 | 16 | 5.0 | 12 |
| Shape | 6.0 | 28 | 5.0 | 20 |
| Upsample | 5.5 | 16 | 5.0 | 12 |
| Texture | 1.0 | 12 | 1.0 | 12 |

共通設定：

| パラメータ | 値 |
| --- | ---: |
| Manual FOV | 55（`0.0`でMoGe推定） |
| Skip Shape Upscale | false |
| Target Face Count | 700,000 |
| Texture Resolution | 4096 |
| Pixal3D Remesh Project Back | 0.3 |
| TRELLIS.2 Remesh Project Back | 0.1 |
| Remesh Smooth Iterations | 12 |
| Normal Map Cage Distance | 0.02 |

## 実装上の判断

### Texture Resolution 4096

公式テンプレートのデフォルトを意図的に維持しています。低VRAM向け設定ではありません。2048や1024への変更でTexture Bake時の負荷が下がる可能性はありますが、VRAM使用量はまだ定量検証していません。

### Normal Map Cage Distance

Cage Distanceは意図的にMath Expressionから`0.02`を供給しています。現行のPrimitive Floatでは値が小数第1位へ丸められ、必要な精度を保持できないためです。

### FOV

- `0.0`以外：手動FOVを使用
- `0.0`：MoGeが推定したFOVを使用

Pixal3Dは入力画像とのピクセル整合を利用するため、カメラ条件の影響を受けやすい可能性があります。初期値55は作者の実験値で、公式推奨値ではありません。

### Skip Shape Upscale

Texture Stageへ渡すShapeと、Mesh Decode経路の両方を迂回させます。高速な比較や、Upscale Stageが形状破綻へ影響しているかを切り分ける用途を想定しています。

## 既知の制約

- VRAMピークと実行時間は系統的に計測していません。
- 収録プリセットは限定的な入力画像から得た値で、全被写体への一般化は未確認です。
- 穴、内部シェル、非多様体形状、変形に不向きなトポロジーが残る場合があります。
- Advanced 3D Preview／Saveと`viewport_state`の形式はFrontend更新の影響を受ける可能性があります。
- 1グラフ内に両モデルのLoaderがあります。実VRAM挙動はComfyUIのモデル解放と起動引数に依存します。
- 同梱の入力画像は性的示唆を含むアニメ画像です。転載先では適切なセンシティブ設定を使用してください。

## トラブルシューティング

### 「3Dプレビュー（詳細）に必須入力 viewport_state がありません」

ComfyUI本体とFrontendを更新してください。古い状態で保存されたAdvanced Previewが残る場合は、現在のFrontend上でノードを作り直すか、通常のRender Previewとこのフローに追加した`Save GLB`を利用してください。

### ノードが見つからない

新しいComfyUI公式3Dノードを使用しています。更新後に再起動し、画面に表示されるComfyUI／Frontendバージョンを確認してください。

### Texture BakeでOOMになる

公式値を維持するためTexture Resolutionは4096です。診断として2048を試せますが、同品質の軽量版という意味ではありません。

## クレジットとライセンス

次の公式テンプレートをベースにしています。

- https://github.com/Comfy-Org/workflow_templates
- https://comfy.org/workflows/920c795f693a-920c795f693a/

公式テンプレートリポジトリはMIT Licenseです。本リポジトリは派生元を明記し、変更内容を文書化しています。モデルWeightと上流プロジェクトには個別のライセンスがあるため、商用利用や再配布前に別途確認してください。

ワークフローの改変部分と文書は[MIT License](LICENSE)で公開します。
