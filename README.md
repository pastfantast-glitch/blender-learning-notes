# Blender 學習筆記：3ds Max → Blender 轉換指南

這份筆記用來持續整理從 **3ds Max 轉換到 Blender** 的學習內容，並延伸到遊戲美術常用的 Unity / Unreal、FBX、UV、Shader、Normal 與拓樸工作流。

> 更新原則：以 3ds Max 使用習慣作為索引，記錄 Blender 對應功能、快捷鍵、介面位置、用途與注意事項。涉及版本差異、FBX、引擎匯入或插件相容性的內容，會區分已確認資訊與待驗證項目。

> 學習方向：以熟悉的 **3ds Max 功能 → Blender 對應功能 → 操作步驟 → 差異 → 注意事項** 為主；目標是建立 Blender 功能對照知識，不預設要替換現有 3ds Max 工作流程，也不強制深入所有進階設定。

> **分類原則**：第一篇只放 3ds Max ↔ Blender 的功能對照與操作差異；第二篇獨立整理法線、烘焙、引擎匯出等技術原理與製作 SOP。內建插件與官方 Extensions 仍集中於第一篇的「內建插件」章節；第三方工具依用途歸檔。


# 第一篇｜3ds Max ↔ Blender 功能對照

## 1. 視圖、狀態與座標

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| `G → Z → Z` | 沿物件 Local Z 軸移動 | Local Transform | 雙擊軸向可切到物件自身軸向 |
| `N` | 開啟 3D Viewport 側邊欄 | Transform / Utility 面板 | 可查看 Location / Rotation / Scale |
| `T` | 顯示 / 隱藏左側工具列 | 工具列概念 | 部分建模工具可用滑鼠操作 |

## 2. 軸心、Reset XForm 與物件管理

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| `Ctrl + A → Rotation & Scale` | Apply Transform | Reset XForm → Collapse | Rotation 回 0、Scale 回 1 |
| `Ctrl + A → Scale` | 套用縮放 | Reset XForm 的 Scale 部分 | 非均勻縮放會影響 Bevel、Sculpt 等操作 |
| 右鍵 → Set Origin | 設定 Origin | Center to Object / Affect Pivot | 可配合 3D Cursor |
| `Shift + S` | Cursor / Selection 吸附 | Pivot / Selection 對齊 | 常用於精確設定 Origin |
| `Ctrl + J` | 合併物件 | Attach | Object Mode |
| `Ctrl + P` | 建立 Parent | Parent / Link | 影響 Transform 層級 |
| `Alt + P` | 解除 Parent | Unlink | 保持世界位置時使用 Clear and Keep Transformation |

## 3. Edge → Curve

對應 Max 的 **Create Shape from Selection**：

1. Edit Mode 選取 Edge。
2. `Shift + D` 複製，右鍵取消位移。
3. `P → Selection` 分離成獨立 Mesh。
4. Object Mode → 右鍵 → Convert To → Curve。
5. Curve Data Properties → Geometry → Bevel → Depth 可產生厚度。

## 4. 網格編輯與拓樸

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| `Alt + S` | Shrink/Fatten | Push | 沿頂點法線方向收縮 / 膨脹 |
| Mesh → Transform → Shrink/Fatten | UI 選單版本 | Push | 不使用快捷鍵時使用 |
| Options → Correct Face Attributes | 幾何移動時補償 UV / Face Corner 資料 | Preserve UVs | 並非所有拓樸操作都能完全無損保持 UV |
| `G → G` | Edge Slide | Edge Slide | 沿現有拓樸滑動 |
| `Alt + J` | Tris to Quads | Quadrify 類似功能 | 可依 UV / Seam / Sharp 等條件限制合併；不是任意模型皆可無損還原 |

## 5. 內建插件 / 官方 Extensions

這一章專門收錄 Blender 內建附加元件、過去隨 Blender 提供的 bundled add-ons，以及目前透過官方 Blender Extensions 平台安裝的工具。

> 規則：只要屬於 Blender 官方隨附／官方 Extensions 體系，就統一放在這一章；第三方商業或外部插件則放在對應主題章節。

### 5.1 LoopTools

LoopTools 是 Blender 常用的 Mesh 建模工具集。它在 **Blender 4.1 以前屬於 bundled add-on**；Blender 4.2 之後改由官方 **Blender Extensions** 平台提供。

#### 安裝／啟用（Blender 4.5）

1. 開啟 **Edit → Preferences**。
2. 左側進入 **Get Extensions**。
3. 搜尋 `LoopTools`。
4. 找到 **LoopTools**，按 **Install**。
5. 安裝完成後確認已啟用；若之後停用，可回到 Preferences 的 Extensions / Add-ons 管理頁重新 Enable。

如果無法連到 Extensions 平台，也可以下載 LoopTools 的 `.zip`，再到 Preferences → Get Extensions 右上選單使用 **Install from Disk** 安裝。

#### Circle：將選取頂點排列成圓形

**LoopTools → Circle** 可以把選取的一圈頂點重新排列成規則圓形，適合製作孔洞、圓形開口與整理圓環拓樸。

操作：

1. 進入 **Edit Mode**。
2. 選取要圓形化的一圈 Vertices。
3. 右鍵 → **LoopTools → Circle**。
4. Circle 會重新排列既有頂點的位置，不會單純為了圓形化而增加頂點數。

LoopTools 另外常用的功能包括 **Relax、Space、Flatten、Bridge**。

## 6. 修改器與法線

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| Displace Modifier | 表面沿法線偏移 | Push Modifier（概念接近） | 並非所有情況都完全等價 |
| `Shift + N` | Recalculate Outside | Unify Normals | Edit Mode |
| Mark Sharp + Smooth by Angle | 控制硬邊 / 平滑 | Smoothing Groups | Blender 4.x 常見工作流 |

### 6.1 3ds Max Smoothing Groups ↔ Blender 4.5 Sharp Edges

| 3ds Max | Blender 4.5 | 備註 |
|---|---|---|
| Smoothing Groups 1～32 | Shade Smooth + Mark Sharp | Blender 主要以面平滑狀態與 Edge Sharp 控制，不以群組編號編輯 |
| Auto Smooth | Shade Auto Smooth / Shade Smooth by Angle | 依相鄰面夾角處理硬邊；版本與實作方式不同 |
| Clear Smoothing Groups | Shade Flat | 效果類似但不完全等價 |
| 手動指定平滑組邊界 | Edit Mode 選 Edge → Ctrl+E → Mark Sharp | 可用 F3 搜尋 Mark Sharp；取消為 Clear Sharp |
| Weighted Normals | Weighted Normal Modifier | 可改善平面／硬表面高光 |

操作：
1. Object Mode → 選模型 → 右鍵 **Shade Smooth**。
2. Edit Mode → 按 2 切換 Edge Select → 選取需要硬邊的 Edge。
3. Ctrl+E → **Mark Sharp**；取消使用 **Clear Sharp**。
4. 若需依角度自動處理，可用 F3 搜尋 **Shade Auto Smooth／Shade Smooth by Angle**，依 Blender 版本檢查具體操作。

**重要差異**：Max 可讓面擁有多個 Smoothing Group，靠群組交集決定平滑；Blender 一般用 Face Smooth／Flat 加 Edge Sharp 決定法線連續性。UV Seam 與 Mark Sharp 是不同資料，不會自動互換。烘焙 Normal Map 前應固定低模法線設定，FBX 匯出後確認目標引擎是否保留法線。

## 7. 查看面數

- 3D Viewport → Viewport Overlays → **Statistics**：顯示 Vertices / Edges / Faces / Tris。
- Status Bar 右鍵 → **Scene Statistics**：在底部狀態列顯示統計。
- 遊戲資產效能預算通常優先確認 **Tris**。

## 8. UV 功能對照與相關插件

| 工具 | 定位 | 適合用途 | 注意事項 |
|---|---|---|---|
| TexTools | 免費 UV 工具集 | Texel Density、對齊、Rectify、Checker | Blender 4.x / 4.5 相容性依版本確認 |
| UVPackmaster 3 | 高效 UV Packing | 大量島嶼、UDIM、Packing | 依官方版本確認相容性 |
| Zen UV | 一體化 UV 工作流 | Unwrap、對齊、Quadrify、UV 管理 | 付費插件 |
| RizomUV Bridge | Blender ↔ RizomUV | 高複雜度專業 UV 工作流 | Bridge 與 RizomUV 版本需匹配 |


### 8.1 依平滑組／硬邊自動展 UV（3ds Max 對照）

**3ds Max**：Unwrap UVW → Flatten Mapping → By Smoothing Groups，依已分配的平滑組切割 UV Islands。

**Blender 4.5**：沒有同名的原生「By Smoothing Groups」選項，通常採兩條路線：

1. **Smart UV Project**：Edit Mode → A 全選 → U → Smart UV Project → 設定 Angle Limit 和 Island Margin。這是**依面與面夾角**自動展開，並非直接讀取既有 Mark Sharp。Angle Limit 越小通常 UV 島越碎。
2. **已有 Sharp Edge 時：Sharp → Seam → Unwrap**。Mark Sharp 控制法線硬邊；Mark Seam 控制 UV 切割，兩者互不等同。少量邊可直接選 Edge → Ctrl+E → Mark Seam；大量已標 Sharp 的邊可用下面的 Python Console 指令複製標記到 Seam，然後 A → U → Unwrap。

```python
import bpy
import bmesh
obj = bpy.context.edit_object  # 須先選取 Mesh 並進入 Edit Mode
bm = bmesh.from_edit_mesh(obj.data)
for edge in bm.edges:
    if not edge.smooth:  # 已標記 Sharp 的邊
        edge.seam = True
bmesh.update_edit_mesh(obj.data)
```

此腳本會**保留既有 Seam**，只額外標記 Sharp Edge 為 Seam。硬邊切割不一定等於理想 UV 切割：圓柱、角色等曲面可能仍須增加或刪減 Seam 以控制拉伸與接縫位置。

參考：[Blender 4.5 UV Operators](https://docs.blender.org/manual/en/4.5/modeling/meshes/editing/uv.html)、[Blender 4.5 Python UV API](https://docs.blender.org/api/4.5/bpy.ops.uv.html)。

### 8.2 Sharp Edge → Seam → Unwrap 的插件選項（Blender 4.5）

對應 3ds Max **Unwrap UVW → Flatten Mapping → By Smoothing Groups**：

| 工具 | 是否直接利用已標記 Sharp Edge | 展開能力 | 注意事項 |
|---|---|---|---|
| **Zen UV** | 是；**Conversion System → Seams by Sharp Edges** | 執行轉 Seam 後使用 **Zen Unwrap**，可依需求 Pack | 適合保留手動 Sharp 規劃；屬第三方付費插件 |
| **Edge to Seam Toolkit** | 是；**Apply Sharp to Seams + Unwrap** | 一鍵轉換與展開 | 據產品文件會清除既有 Seams 與 UV Maps，操作前須備份 |
| **Zeeks Auto UV Unwrap** | 不一定；主要依幾何銳利轉折判斷 | 自動建立 Seam、Unwrap；官方平台標示 Blender 4.2+ | 非完全等同既有 Sharp 標記轉換 |

**Zen UV 推薦步驟**：安裝相容 Blender 4.5 的版本並啟用 → Edit Mode 設定 Mark Sharp → Zen UV 的 Conversion System **Seams by Sharp Edges** → 檢查紅色 Seam → **Zen Unwrap** → Pack Islands。另有「Auto Seams & Unwrap」可從幾何自動判斷並展 UV，但不等同僅使用既有 Sharp 標記。

**不裝插件的替代方案**：先選取所有已標記 Sharp 的 Edge，然後 Mark Seam → Unwrap；或參照上一節的 BMesh Python 腳本批次將 Sharp 複製到 Seam。無論使用哪一種方法，必要時仍需為圓柱等形狀補充 Seam。

參考來源：
- [Zen UV 官方 Unwrap／Conversion System](https://zenmastersteam.github.io/Zen-UV/latest/unwrap/)
- [Edge to Seam Toolkit 官方說明](https://superhivemarket.com/products/edge-to-seam-toolkit--hard-surface-uv-add-on/docs)
- [Zeeks Auto UV Unwrap 官方 Extensions](https://extensions.blender.org/add-ons/zeeks-auto-uv-unwrap/)

### 8.3 RizomUV：利用硬邊資訊進行 Auto Seams 與 Unfold

**RizomUV 可以處理這個需求，先前列舉 UV 插件時不應忽略它。** RizomUV 的 **Auto Seams → Sharpest Angles** 適合硬表面模型，可分析幾何角度、Quad 拓樸及 User Vertex Normals 等資訊，自動決定接縫並 Unfold／Pack。RizomUV 更新紀錄亦提到 Auto Seams 的法線來源設定。

**重要區分**：
- 幾何夾角、Blender **Mark Sharp** 標記和傳輸後的 FBX 使用者法線不完全等同。直接送進 RizomUV 的 Auto Seams **不保證逐條採用 Blender 已標記 Sharp Edge**。
- 若要求嚴格沿用 Blender 的 Sharp，較可靠的方式是先在 Blender 把 Sharp 轉為 **Seam**（見 8.1），再以支援 Seam 傳送的 Bridge 進行 Unfold／Pack，或使用支援 Sharp Selection Sync 的 Bridge。

**橋接選擇**：
- [RizomUV Bridge（Superhive）](https://superhivemarket.com/products/rizom-uv-bridge/docs)：支援自動切割、Unfold、Pack、回傳 UV 與傳送 Blender Seam（要檢查 Auto Seams／Cut by My Seams 設定）；需搭配獨立 RizomUV。
- [B2RUVL](https://tituslvr.github.io/B2RUVL/)：文件列出 Sharp／Seam 邊選取同步、Sharp_Edges Lua 腳本；**要求 Blender 5.0+（Windows）**，不適合作為 Blender 4.5 直接安裝的建議。

**Max 對照**：3ds Max 的「By Smoothing Groups」是直接依群組邊界切 UV；RizomUV 的 Sharpest Angles 是自動分析法線／幾何資訊，而非完全同義。若目的為準確沿用 Blender Mark Sharp，採 **Sharp → Seam → Bridge 到 RizomUV → Unfold → Pack → 回傳 Blender**。

參考：[RizomUV 官方算法](https://www.rizomuv.com/c-library/)、[RizomUV 更新紀錄](https://www.rizomuv.com/changelog/)、[RizomUV Bridge 文件](https://superhivemarket.com/products/rizom-uv-bridge/docs)。

## 9. 3ds Max → Blender 快速索引

| 3ds Max | Blender |
|---|---|
| Reset XForm | `Ctrl + A → Rotation & Scale` |
| Push | `Alt + S`；依需求使用 Displace Modifier |
| Preserve UVs | Options → Correct Face Attributes |
| Create Shape from Selection | `Shift+D → P Selection → Convert to Curve` |
| Attach | `Ctrl + J` |
| Unlink Parent | `Alt + P → Clear and Keep Transformation` |
| Show Statistics | Viewport Overlays → Statistics |
| Unify Normals | `Shift + N` |
| Tris to Quads | `Alt + J` |
| 將一圈頂點整理成圓形 | 內建插件 → LoopTools → Circle |

## 10. Bake to Texture ↔ Blender Cycles Bake

### 10.1 功能與輸出元素對照

3ds Max **本身就能將材質／程序化紋理烘焙為圖片**，不需要為了 Base Color 烘焙而必須換到 Blender。新版 3ds Max 可從 **Rendering → Bake to Texture** 開始；舊版常使用 **Render To Texture（RTT）**。實際 Bake Elements 的名稱與可用功能依 3ds Max 版本、材質及渲染器而異。

| 目標 | 3ds Max | Blender Cycles |
|---|---|---|
| 材質顏色 → Base Color | Bake to Texture 的 Albedo／Base Color 類輸出元素（視版本／渲染器而定） | Diffuse：只勾 Color，關閉 Direct／Indirect |
| 程序化材質 → 圖片 | Bake to Texture，確認節點與渲染器支援 | 將目標顏色接到 Emission，再使用 Emit Bake |
| 高模 → 低模 Normal | Projection／Bake to Texture | Normal + Selected to Active；依需求使用 Cage |
| AO | 對應 AO 烘焙元素（依渲染器而定） | AO Bake |
| 多材質 → 單張貼圖 | 先規劃共用 UV 與輸出目標，再 Bake | 共用 UV／目標 Image Texture，逐材質設定接收節點 |

**Material Conversion 不等於 Texture Baking**：將 V-Ray／Arnold 等材質轉成其他材質類型，並不代表已輸出 Base Color 圖片。烘焙純色時須排除光照、陰影、AO、反射等非 Base Color 資訊。第三方渲染器的特殊節點未必能被所有 Bake Element 正確輸出。

## 11. 功能對照待補充

- Symmetry ↔ Mirror Modifier
- Relax / Conform / Shrinkwrap 對照
- Pivot / Working Pivot / Transform Orientation
- Edit Poly 常用指令對照
- Unwrap UVW ↔ Blender UV Editor 深入操作
- 第三方插件版本相容性總表
- 建立可直接套用的 Unity / Unreal FBX Export SOP

# 第二篇｜3D 技術研究與製作流程

> 此篇記錄通用 3D 製作技術、判斷依據與引擎工作流程，不以 Max 與 Blender 的指令對照為主。

## 12. 法線與硬表面技術

### 12.1 法線傳導（Normal Transfer / Data Transfer）

**用途**：將來源 Mesh 的 Custom Normals 傳遞給目標 Mesh，改善目標物件的受光表現，**不會改變幾何外形**。Blender 4.5 主要使用 **Data Transfer Modifier → Custom Normals**；對照 3ds Max 的 Explicit/Vertex Normal 編輯與法線複製概念。

**適合使用時機**：
- 葉片、Hair Cards 等平面構成的模型：從球形／橢球形代理模型取得平滑法線，減少受光破碎。
- 貼在角色曲面上的附加 Mesh：繼承主體近似法線，減少接合處不必要的明暗斷層。
- NPR／Toon 材質：有意識地調整 Vertex Normal 驅動的受光分布；必須確認 Shader 是否採用該法線。

**不需要的情況**：一般平滑使用 Shade Smooth；硬邊使用 Mark Sharp；改善大平面硬表面高光可先測試 Weighted Normal；高低模雕刻細節需 Normal Map Bake，並非 Normal Transfer。

**Blender 4.5 步驟**：
1. 準備受光理想的 **Source** 模型，與待修正的 **Target** 模型。
2. 選取 Target → Modifier Properties → Add Modifier → **Data Transfer**。
3. Source 指定來源 Mesh，啟用 **Custom Normals**；必要時按 **Generate Data Layers**。
4. Mapping 可從 **Nearest Face Interpolated** 開始測試，再依模型拓樸、距離與投射方向調整；必要時使用 Max Distance、Mix Factor、Vertex Group 限制影響。
5. 檢查硬邊、鏡面高光及目標引擎呈現，確認後再套用修改器。

**對照區分**：Weighted Normal 是依自身面積／角度重新計算法線；Data Transfer 是從別的 Mesh 複製／插值法線。傳導會改變頂點／面角法線的 Shading，可能導致不自然的高光；Normal Map Bake、UV Seam 與 Mark Sharp 是不同概念。若輸出 FBX 給 Unity／Unreal，應檢查自訂法線是否確實匯出、引擎是否重新計算法線，以及最終 Normal Map 烘焙是否與法線一致。

參考：[Blender 4.5 Data Transfer Modifier](https://docs.blender.org/manual/en/4.5/modeling/modifiers/modify/data_transfer.html)、[Blender Normal Edit Modifier](https://docs.blender.org/manual/en/4.5/modeling/modifiers/normals/normal_edit.html)。

### 12.2 法線傳導與 High Poly → Low Poly Normal Bake 的關係

**法線傳導（Data Transfer → Custom Normals）可以在烘焙前用來調整低模頂點／面角法線，但不會自行將高模雕刻細節烘焙成貼圖。** Tangent Space Normal Map 依低模的法線與切線基底編碼；更改低模法線可能改變烘焙結果與引擎呈現。

**適用案例**：特殊低面數曲面、貼附於角色表面的細節 Mesh、Hair Cards 或需要特殊 NPR 受光方向的資產。一般角色素體通常使用適當的 Shade Smooth、Mark Sharp、合理 UV 及 Cage 即可，不需要強制傳導法線。

**推薦順序**：
1. 完成 Low Poly 拓樸與 UV。
2. 決定低模 Smooth／Sharp 與必要的 Custom Normals；若有需要，使用 Data Transfer 從參考 Mesh 傳導。
3. 固定低模最終法線狀態，再用 High Poly → Low Poly（Selected to Active）烘焙 Tangent Space Normal Map。
4. 依需求使用 Cage 或 Ray Distance，檢查硬邊、UV 接縫及投射瑕疵。
5. 匯出 Low Poly + Normal Map 到 Unity／Unreal，確認引擎的法線／切線匯入與烘焙時一致。

**注意**：烘焙後再修改低模 Sharp、Custom Normals、UV 切割或引擎重新計算法線，可能造成貼圖與切線基底不一致，需要重烘。法線傳導不是改善高低模幾何投射錯誤的萬用方法；Cage 和合理的 UV／硬邊分割仍很重要。

參考：[Blender 4.5 Cycles Baking](https://docs.blender.org/manual/en/4.5/render/cycles/baking.html)、[Blender 4.5 Data Transfer](https://docs.blender.org/manual/en/4.5/modeling/modifiers/modify/data_transfer.html)。

### 12.3 硬表面低模：單段 Bevel（原一條硬邊 → 兩條邊 + 一個斜面）是否還要切 Sharp？

**先釐清幾何**：原本 90° 的單條硬邊，執行 **Bevel Segments = 1** 後產生一個斜面、兩條交界邊；**不是 Segments = 2** 的兩段倒角。這個斜面具有實際表面朝向，可提供實體高光，但輪廓仍只有一段直線倒角，並非圓弧。

**結論：不一定需要切平滑組／Mark Sharp。** 優先測試 **Shade Smooth**，若大平面法線受倒角牽動、產生奇怪漸層，可嘗試 **Weighted Normal**；必要時搭配 Bevel Modifier 的 **Harden Normals / Face Strength**。它們會調整 Shading，但不會增加幾何弧度。

- 想要柔和的高光過渡：通常保留 Smooth 並檢查 Weighted Normal 效果。
- 想保留大平面的平直法線，或需要明確折角：可以在斜面兩側 **Mark Sharp**，但高光會更不連續。
- **High → Low Normal Map Bake**：在烘焙前決定法線／Sharp／UV 與必要的 Weighted Normal。硬邊對應處應切 UV Island，減少切線空間接縫問題；反之 UV Seam 不一定要切硬邊。
- 最後以高光、貼圖與 Unity／Unreal 中實際法線表現作判斷。**不是所有單段倒角都一定需要切 Sharp，也不是一定不能切。**

**推薦順序**：Bevel Segments = 1 → Shade Smooth → 檢查大平面與斜面高光 → 視情況 Weighted Normal → 必要時 Mark Sharp + UV Split → Normal Bake。

參考：[Marmoset Baking Tips & Tricks](https://docs.marmoset.co/docs/tips-tricks/)、[Blender Bevel Modifier](https://docs.blender.org/manual/en/4.5/modeling/modifiers/generate/bevel.html)。

### 12.4 Weighted Normal 是否需要輸出到 Unity／Unreal？

**需要保留的是 Weighted Normal 計算後的 Custom Normals，不是 Modifier 本身。** FBX 只承載最終 Mesh／Normals，不會把 Blender 的 Weighted Normal Modifier 原樣帶到遊戲引擎。

**Blender 4.5 FBX 輸出建議**：
1. 保留 Blender 檔案中的 Weighted Normal Modifier，以便日後非破壞性調整。
2. File → Export → FBX；Geometry → **Apply Modifiers** 開啟，輸出修改器評估後的模型。
3. Geometry → **Smoothing: Normals Only**，適用於支援自訂分離法線的匯入器。
4. 將匯出 FBX 重新匯入測試，確認大平面、Bevel 高光以及自訂法線結果。

**Unity**：FBX Model Import Settings → **Normals: Import**，避免引擎用 Calculate 覆蓋 Blender 的法線；**Tangents: Calculate Mikktspace** 可作為一般切線空間 Normal Map 工作流程的起點，但仍需與烘焙端的切線空間一致。Unreal 也應選擇保留／匯入自訂法線的設定，實際名稱依引擎版本而異。

**Normal Bake 一致性**：在 High → Low Bake 之前定案 Weighted Normal、Sharp、UV 與三角化；最後匯入引擎的低模法線應盡量和 Bake 時相同。若事後關閉 Weighted Normal 或在引擎重新計算法線，Normal Map 可能出現高光、陰影、接縫異常。

參考：[Blender 4.5 FBX Manual](https://docs.blender.org/manual/en/4.5/addons/import_export/scene_fbx.html)、[Unity Model Import Settings](https://docs.unity.com/en-us/engine/7000.0/manual/assets-and-media/asset-types/models/importing/fbximporter-model)。

## 13. Texture Baking 技術與流程

以下以 Blender 4.5 Cycles 示範，技術原則也適用於其他 DCC 的 High/Low 烘焙工作。

### 13.1 基本烘焙：材質顏色 → 圖片

1. 模型先完成 UV 展開，並確認 UV 不會不必要地重疊。
2. Render Properties → Render Engine 設為 **Cycles**。
3. 在模型材質的 Shader Editor 新增 **Image Texture**，按 New 建立目標圖片（例如 2048 × 2048）。**點選 Image Texture 節點，使其成為 Active Node**；無須連接到 BSDF。多材質物件須確保每個相關材質都有正確的目標圖片節點且為 Active。
4. Render Properties → Bake：選 **Diffuse**，只勾 **Color**，取消 **Direct**、**Indirect**，以避免將光照烘焙進顏色。
5. 選取模型按 **Bake**，完成後在 Image Editor → Image → **Save As** 儲存圖片。

若要精確輸出程序化節點計算後的顏色，可以將結果接到 Emission，改用 **Emit** 烘焙；要注意色彩空間及輸出設定。

### 13.2 高模 → 低模 Normal Map

1. 高模與低模正確對位；低模先展好 UV。
2. 在低模材質中新增並選取目標 **Image Texture** 節點（Normal 圖片建議用 Non-Color 解讀）。
3. Bake Type 設為 **Normal**，Space 設 **Tangent**；勾選 **Selected to Active**。
4. Object Mode **先選高模，最後選低模**，確保低模為 Active Object。
5. 調整 **Max Ray Distance**，必要時使用 Cage 避免投射錯誤，再按 **Bake**。
6. 檢查 UV 接縫、法線方向與投射結果，並儲存圖片。

### 13.3 注意事項與排錯

- 烘焙結果空白：確認使用 Cycles、目標 Image Texture 節點為 Active，並檢查 UV。
- 黑邊／接縫：檢查 UV Island 間距與 **Margin**。
- Normal 細節錯亂：檢查高低模對位、Selected to Active、Ray Distance、Cage。
- Blender 通常使用 OpenGL（Y+）切線空間 Normal；若目標引擎或專案使用 DirectX（Y−）慣例，須確認是否反轉綠色通道。
- Bake 完成後仍須 **Image → Save As**，避免只留在暫存圖片中。

### 13.4 額外 Bake 插件（Blender 4.5 學習參考）

這些屬於第三方插件／第三方官方平台 Extensions，不是 Blender 核心內建功能。

| 插件 | 用途 | Blender 4.5 注意事項 |
|---|---|---|
| TexTools | UV 與多種 Bake 模式；有 3ds Max 與 Blender 版本 | GitHub 標示 Blender 3.2+，仍須核對最新版 |
| SimpleBake | 簡化 PBR／程序化材質烘焙 | 開發者表示 2.3.4 維持 4～4.5 相容，但正式支援重心已轉向 5+ |
| Bake Wrangler | 節點式烘焙、批次與進階輸出 | 2.0.4 官方文件要求 Blender 5.0.1+；4.5 須查舊版 |
| BakeLab | 自動 UV、圖片與烘焙 | 舊版對應 2.79，新版需另查 |
| The Better Baker | 官方 Extensions 平台上的第三方 PBR 烘焙插件 | 依 Extensions 頁面核對版本 |

安裝前先確認相容性與來源；從 ZIP 安裝可在 Preferences 的 Extensions／Add-ons 管理介面選 Install from Disk，安裝後再啟用。官方 Extensions 平台上架不代表 Blender 核心內建。

參考：
- https://github.com/erox455/textools-blender
- https://www.toohey.co.uk/SimpleBake/releasenotes4.html
- https://bake-wrangler.readthedocs.io/en/latest/installation.html
- https://extensions.blender.org/tag/add-ons/bake/

## 14. Shader / Normal Map 技術

### 反轉 Normal Map 綠色通道

如果需要反轉法線貼圖的 Y / Green 通道：

1. Image Texture → **Separate Color**。
2. R 保留。
3. G 執行 `1 - G`。
4. B 保留。
5. 使用 **Combine Color** 合併 RGB。
6. 輸出接到 **Normal Map** 節點的 Color。

注意：如果使用 Invert Color，要避免把整個 RGB 一起反轉。

## 15. Unity / Unreal：FBX、單位與座標技術

這一區是舊 Gemini 筆記中最需要謹慎處理的部分。FBX 單位、Blender Exporter、Unity Model Importer、Unreal Importer 與 Axis Conversion 會互相影響，不應把單一設定視為所有專案的固定規則。

### Blender 基準

- 常見工作流：Metric、Unit Scale = `1.0`。
- 輸出前確認 Rotation / Scale，尤其是非均勻縮放。
- 外部 FBX 匯入 Blender 後若有 Empty / Parent，先確認用途再決定是否解除。

### Unity 0.01 / 90° 問題

- 區分 **FBX Importer Scale Factor / Convert Units** 與 **Hierarchy GameObject Transform Scale**。
- 可建立 Blender `1m × 1m × 1m` Cube，與 Unity 內建 Cube 並排驗證世界尺寸。
- 若世界尺寸正確且場景 GameObject Scale = `1,1,1`，不要只因 Importer 內部顯示 `0.01` 就持續修改 Blender。
- 若尺寸正確但 Root Rotation 有 ±90°，再檢查 Axis Conversion、Forward / Up 與引擎 Import 設定。

### 舊筆記中需要避免當成絕對規則的說法

- Apply Transform 一定能讓 Unity / Unreal Root 變成 `0,0,0`。
- Forward / Up 絕對不能修改，否則一定 Double Conversion。
- FBX 是公分，因此 Unity 一定會讓 Transform 變成 `0.01`。
- Unity Transform 不是 `1,1,1` 就一定有問題。

---

**持續更新中。** 新增內容依「功能對照」或「技術研究」分類，避免同一知識在多個章節重複記錄。
