# Blender 學習筆記：3ds Max → Blender 轉換指南

這份筆記用來持續整理從 **3ds Max 轉換到 Blender** 的學習內容，並延伸到遊戲美術常用的 Unity / Unreal、FBX、UV、Shader、Normal 與拓樸工作流。

> 更新原則：以 3ds Max 使用習慣作為索引，記錄 Blender 對應功能、快捷鍵、介面位置、用途與注意事項。涉及版本差異、FBX、引擎匯入或插件相容性的內容，會區分已確認資訊與待驗證項目。

> 學習方向：以熟悉的 **3ds Max 功能 → Blender 對應功能 → 操作步驟 → 差異 → 注意事項** 為主；目標是建立 Blender 功能對照知識，不預設要替換現有 3ds Max 工作流程，也不強制深入所有進階設定。

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

## 8. UV 工作流與第三方插件

| 工具 | 定位 | 適合用途 | 注意事項 |
|---|---|---|---|
| TexTools | 免費 UV 工具集 | Texel Density、對齊、Rectify、Checker | Blender 4.x / 4.5 相容性依版本確認 |
| UVPackmaster 3 | 高效 UV Packing | 大量島嶼、UDIM、Packing | 依官方版本確認相容性 |
| Zen UV | 一體化 UV 工作流 | Unwrap、對齊、Quadrify、UV 管理 | 付費插件 |
| RizomUV Bridge | Blender ↔ RizomUV | 高複雜度專業 UV 工作流 | Bridge 與 RizomUV 版本需匹配 |

## 9. Shader / Normal Map

### 反轉 Normal Map 綠色通道

如果需要反轉法線貼圖的 Y / Green 通道：

1. Image Texture → **Separate Color**。
2. R 保留。
3. G 執行 `1 - G`。
4. B 保留。
5. 使用 **Combine Color** 合併 RGB。
6. 輸出接到 **Normal Map** 節點的 Color。

注意：如果使用 Invert Color，要避免把整個 RGB 一起反轉。

## 10. Blender ↔ Unity / Unreal：FBX、單位與座標

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

## 11. 3ds Max → Blender 快速索引

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


## 13. 貼圖烘焙（Texture Baking / Render To Texture）

對應 3ds Max 的 Render To Texture。以下以 Blender 4.5 的 Cycles 為基準。

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


### 13.4 與 3ds Max Bake to Texture 的對照

3ds Max **本身就能將材質／程序化紋理烘焙為圖片**，不需要為了 Base Color 烘焙而必須換到 Blender。新版 3ds Max 可從 **Rendering → Bake to Texture** 開始；舊版常使用 **Render To Texture（RTT）**。實際 Bake Elements 的名稱與可用功能依 3ds Max 版本、材質及渲染器而異。

| 目標 | 3ds Max | Blender Cycles |
|---|---|---|
| 材質顏色 → Base Color | Bake to Texture 的 Albedo／Base Color 類輸出元素（視版本／渲染器而定） | Diffuse：只勾 Color，關閉 Direct／Indirect |
| 程序化材質 → 圖片 | Bake to Texture，確認節點與渲染器支援 | 將目標顏色接到 Emission，再使用 Emit Bake |
| 高模 → 低模 Normal | Projection／Bake to Texture | Normal + Selected to Active；依需求使用 Cage |
| AO | 對應 AO 烘焙元素（依渲染器而定） | AO Bake |
| 多材質 → 單張貼圖 | 先規劃共用 UV 與輸出目標，再 Bake | 共用 UV／目標 Image Texture，逐材質設定接收節點 |

**Material Conversion 不等於 Texture Baking**：將 V-Ray／Arnold 等材質轉成其他材質類型，並不代表已輸出 Base Color 圖片。烘焙純色時須排除光照、陰影、AO、反射等非 Base Color 資訊。第三方渲染器的特殊節點未必能被所有 Bake Element 正確輸出。

### 13.5 額外 Bake 插件（Blender 4.5 學習參考）

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

## 12. 待補充

- Symmetry ↔ Mirror Modifier
- Relax / Conform / Shrinkwrap 對照
- Pivot / Working Pivot / Transform Orientation
- Edit Poly 常用指令對照
- Unwrap UVW ↔ Blender UV Editor 深入操作
- 第三方插件版本相容性總表
- 建立可直接套用的 Unity / Unreal FBX Export SOP

---

**持續更新中。** 後續 Blender 問答中，適合長期保留且已確認的內容會整理進這份筆記。