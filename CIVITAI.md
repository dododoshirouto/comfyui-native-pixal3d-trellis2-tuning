# Civitai publication kit

This file contains copy for publishing the workflow as a Civitai **Workflow** resource. Replace fields marked `[ADD ...]` only when the corresponding measurement or image is available.

## Recommended title

**Native Pixal3D & TRELLIS.2 Tuning Lab — Official ComfyUI Nodes**

Alternative shorter title:

**Pixal3D / TRELLIS.2 Native Tuning Workflow**

## Suggested category and labels

- Resource type: Workflow
- Base model/category: Pixal3D, TRELLIS.2, Image to 3D
- Intended use: Local generation, parameter comparison, PBR asset generation
- Content label for the supplied example: Mature / suggestive
- Commercial-use label: Do not infer this from the workflow license; model licenses must be checked separately

## Short description

### English

A tuning and comparison variant of ComfyUI's official native Pixal3D & TRELLIS.2 image-to-3D workflow. It exposes separate CFG, steps and seeds for both models, plus FOV override, Shape Upscale bypass, remesh controls, PBR map previews and GLB export—without requiring a third-party generation node pack.

### 日本語

ComfyUI公式ネイティブ版Pixal3D／TRELLIS.2フローを、比較・調整向けに再構成したワークフローです。両モデル別のCFG・Steps・Seedに加え、FOV、Shape Upscale迂回、Remesh、PBR Map確認、GLB保存を操作できます。サードパーティ製生成ノードは不要です。

## Full Civitai description — English

### What this is

This is an experimental **parameter-surfaced variant** of ComfyUI's official **Pixal3D & TRELLIS.2: Image to Model** template.

The official graph already provides a complete native image-to-3D pipeline. This version does not replace its models or inference implementation. Instead, it reorganizes the graph into a practical tuning lab so that Pixal3D and TRELLIS.2 can keep separate settings and be compared from the same input.

This workflow is best understood as a middle layer between:

1. using the official template as a mostly fixed reference graph; and
2. installing a large third-party 3D node pack for deeper experimental features.

### Verified changes from the official JSON

- Shared, Pixal3D and TRELLIS.2 control groups
- Per-model routing for Structure, Shape, Upsample and Texture CFG/steps
- Centralized Structure, Shape and Texture seeds
- Manual camera FOV, with `0.0` selecting MoGe estimation
- Optional Shape Upscale bypass
- Per-model Remesh Project Back values
- Remesh smoothing iterations
- Decimation mode and target face count
- Five added Render Mesh checkpoints across the generation/post-processing pipeline
- Standard GLB output alongside the advanced native 3D save path

The native generation stages, 4096 texture resolution, PBR map baking/previews, vertex-color branch and Advanced 3D nodes already exist in the official workflow. They are retained, not claimed as additions.

### Included experimental presets

| Stage | Pixal3D | TRELLIS.2 |
| --- | --- | --- |
| Structure | CFG 6.0 / 16 steps | CFG 5.0 / 12 steps |
| Shape | CFG 6.0 / 28 steps | CFG 5.0 / 20 steps |
| Upsample | CFG 5.5 / 16 steps | CFG 5.0 / 12 steps |
| Texture | CFG 1.0 / 12 steps | CFG 1.0 / 12 steps |

These values are starting points from limited testing. They are not presented as “best settings,” and they may behave differently with other subjects, camera angles or ComfyUI versions.

### Important implementation details

- **Texture Resolution is 4096 intentionally.** This preserves the official template default; it is not a low-VRAM preset.
- **Normal Map Cage Distance uses a Math Expression node intentionally.** The current Primitive Float widget rounds the value to one decimal place, so Math Expression is used to preserve `0.02`.
- **FOV `0.0` means automatic.** Any other value overrides MoGe's estimated FOV.
- **Skip Shape Upscale** reroutes both the texture-stage shape input and decoded mesh path.

### Requirements

- A recent ComfyUI build with native Pixal3D/TRELLIS.2 and native 3D nodes
- The same model files required by the official workflow template
- No third-party generation node pack

Confirmed environment:

- ComfyUI 0.34.0
- ComfyUI Frontend 1.51.10
- Workflow Templates 0.11.55
- Linux
- RTX 5070 Ti 16 GB
- Python 3.13.14
- PyTorch 2.14.0.dev20260723+cu132

Compatibility outside this environment is not yet confirmed.

### Usage

1. Load the workflow JSON.
2. Replace the image in `Load Image`.
3. Set **Model** to `false` for Pixal3D or `true` for TRELLIS.2.
4. Start with the included values, then change one stage at a time.
5. Inspect the geometry/render and baked-map previews.
6. Retrieve the GLB from `ComfyUI/output/3d/ComfyUI`.

### Limitations

- No systematic peak-VRAM or speed benchmark is claimed yet.
- Image-to-3D results are not guaranteed to be watertight, manifold, animation-ready or print-ready.
- The official native pipeline and frontend are developing quickly; serialized Advanced Preview state may require node recreation after an update.
- The bundled presets are experimental and subject-dependent.

### Credits

Derived from Comfy-Org's official workflow template:

- https://comfy.org/workflows/920c795f693a-920c795f693a/
- https://github.com/Comfy-Org/workflow_templates

Please review the licenses of Pixal3D, TRELLIS.2 and each model weight separately before commercial use.

Feedback is most useful when it includes the selected model, input type, changed values, GPU/VRAM and whether the problem appeared during Structure, Shape, Upsample, Remesh or Texture Bake.

## Civitai掲載本文 — 日本語

### これは何か

ComfyUI公式の **Pixal3D & TRELLIS.2: Image to Model** をベースに、モデル比較と調整のための操作UIを追加した実験用ワークフローです。

モデルや推論処理を独自実装へ置き換えたものではありません。公式ネイティブパイプラインを保ったまま、グラフ内に散らばっていた値をPixal3D用・TRELLIS.2用・共通設定へ分け、同じ入力画像から両者を比較しやすくしています。

位置づけとしては、次の中間です。

1. 公式テンプレートをほぼ固定状態で使う
2. 大規模なサードパーティ製3Dノードパックを導入して拡張する

公式ノードだけで、調整可能性を一段増やすことを目的にしています。

### 公式JSONとの比較で確認した変更点

- 共通／Pixal3D／TRELLIS.2の操作グループ
- Structure／Shape／Upsample／TextureのCFG・Stepsをモデル別に切り替える配線
- Structure／Shape／Texture Seedの中央集約
- 手動FOVとMoGe推定の切替
- Shape Upscaleの迂回
- モデル別Remesh Project Back
- Remesh Smooth Iterations
- Decimate Mode／Target Face Count
- 生成・後処理の5地点にRender Mesh Previewを追加
- Advanced Saveに加え、通常版Save GLB

公式ネイティブ生成Stage、4096 Texture、PBR MapのBake／Preview、Vertex Color経路、Advanced 3Dノードは公式版にも存在します。このフローでも維持していますが、追加機能としては扱いません。

### 初期値

| Stage | Pixal3D | TRELLIS.2 |
| --- | --- | --- |
| Structure | CFG 6.0／16 steps | CFG 5.0／12 steps |
| Shape | CFG 6.0／28 steps | CFG 5.0／20 steps |
| Upsample | CFG 5.5／16 steps | CFG 5.0／12 steps |
| Texture | CFG 1.0／12 steps | CFG 1.0／12 steps |

これらは限定的な入力で調整した出発点であり、Best Settingsや全被写体への最適値ではありません。

### 実装上の補足

- Texture Resolution 4096は公式テンプレートの初期値を維持したものです。低VRAMプリセットではありません。
- Normal Map Cage DistanceはMath Expressionから`0.02`を入力します。Primitive Floatでは小数第1位へ丸められるため、精度を保持するための意図的な構成です。
- FOVを`0.0`にするとMoGe推定、0以外にすると手動値を使用します。
- Skip Shape UpscaleはTexture StageとMesh Decodeの両経路を切り替えます。

### 動作確認

- ComfyUI 0.34.0
- ComfyUI Frontend 1.51.10
- Workflow Templates 0.11.55
- Linux
- RTX 5070 Ti 16GB
- Python 3.13.14
- PyTorch 2.14.0.dev20260723+cu132

この環境以外の互換性は未確認です。

### 使い方

1. JSONをComfyUIへ読み込む。
2. Load Imageを差し替える。
3. Modelを`false`でPixal3D、`true`でTRELLIS.2にする。
4. 最初は収録値で実行し、その後Stageを一つずつ変更する。
5. Geometry Renderと各Texture Mapを確認する。
6. `ComfyUI/output/3d/ComfyUI`からGLBを取得する。

### 制約

- VRAMピークと速度の系統的なベンチマークはまだありません。
- Watertight、Manifold、Animation-ready、Print-readyは保証しません。
- 公式ノードとFrontendは更新が続いており、Advanced Previewの保存状態が変わった場合はノードの再作成が必要になる可能性があります。
- 収録値は被写体依存の実験値です。

### クレジット

Comfy-Org公式テンプレートからの派生です。

- https://comfy.org/workflows/920c795f693a-920c795f693a/
- https://github.com/Comfy-Org/workflow_templates

商用利用前にはPixal3D、TRELLIS.2、各Weightのライセンスを個別に確認してください。

不具合報告では、選択モデル、入力の種類、変更した値、GPU／VRAM、Structure・Shape・Upsample・Remesh・Texture Bakeのどこで問題が出たかを書いてもらえると比較しやすいです。

## Suggested tags

```text
comfyui
workflow
image-to-3d
3d
pixal3d
trellis2
trellis-2
pbr
glb
local-ai
official-nodes
parameter-tuning
mesh
texture-baking
```

Avoid tags such as `low-vram`, `optimized`, `best-settings`, `game-ready`, or `print-ready` until they are supported by measurements or validation.

## Recommended media order

1. A neutral workflow-overview screenshot showing the three parameter groups
2. Side-by-side Pixal3D and TRELLIS.2 turntable or matching camera renders
3. A close-up geometry comparison
4. Base Color / Normal / Roughness / Metallic / AO sheet
5. The supplied input reference, marked Mature if used
6. Optional screenshot of the saved GLB in Blender

Do not use the supplied suggestive image as the unblurred public cover. It would make the listing appear to be character/NSFW content rather than a technical workflow and may reduce general discovery.

## Recommended version name

**v0.1.0 — Initial native tuning release**

Version notes:

```text
- Based on ComfyUI's official native Pixal3D & TRELLIS.2 workflow
- Added separate Pixal3D and TRELLIS.2 CFG/step presets
- Added stage-specific seed controls
- Added manual/MoGe FOV switching
- Added Shape Upscale bypass
- Exposed remesh, smoothing, decimation and face-count controls
- Added five Render Mesh checkpoints and standard GLB saving
- Preserved the official 4096 texture default and 0.02 normal-map cage distance
```

## Publication checklist

- [ ] Upload `workflows/native_pixal3d_trellis2_tuning.json`
- [ ] Select Workflow as the resource type
- [ ] Mark the supplied reference/result media Mature where required
- [ ] Use a neutral workflow screenshot as the cover
- [ ] Add at least one same-seed or clearly documented comparison
- [ ] State that presets are experimental, not “best settings”
- [ ] State that 4096 is the official texture default, not a low-VRAM claim
- [ ] Include the confirmed ComfyUI/frontend/template versions
- [ ] Link the GitHub repository as the canonical source
- [ ] Link and credit the official ComfyUI workflow
- [ ] Do not enable commercial permissions solely because this repository uses MIT; check model licenses separately
- [ ] Add VRAM/time figures only after measuring them
