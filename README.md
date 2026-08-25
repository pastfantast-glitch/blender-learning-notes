# Blender 學習筆記：3ds Max → Blender 轉換指南

這份筆記用來持續整理從 **3ds Max 轉換到 Blender** 的學習內容，並延伸到遊戲美術常用的 Unity / Unreal、FBX、UV、Shader、Normal 與拓樸工作流。

> 更新原則：以 3ds Max 使用習慣作為索引，記錄 Blender 對應功能、快捷鍵、介面位置、用途與注意事項。涉及版本差異、FBX、引擎匯入或插件相容性的內容，會區分已確認資訊與待驗證項目。

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

### LoopTools：官方擴充建模工具

LoopTools 是 Blender 常用的 Mesh 建模工具集。它在 **Blender 4.1 以前屬於 bundled add-on**；Blender 4.2 之後改由官方 **Blender Extensions** 平台提供。Blender 4.5 可直接從 Preferences 安裝。

#### 安裝／啟用（Blender 4.5）

1. 開啟 **Edit → Preferences**。
2. 左側進入 **Get Extensions**。
3. 搜尋 `LoopTools`。
4. 找到 **LoopTools**，按 **Install**。
5. 安裝完成後確認已啟用；若暫時停用，也可在 Preferences 的 Extensions / Add-ons 管理頁重新 Enable。

如果電腦無法連到 Extensions 平台，也可以從 Blender Extensions 網站下載 LoopTools 的 `.zip`，再到 Preferences → Get Extensions 右上選單使用 **Install from Disk** 安裝。

#### Circle：將選取頂點排列成圓形

**LoopTools → Circle** 可以把選取的一圈頂點重新排列成規則圓形，適合製作孔洞、圓形開口與整理圓環拓樸。

操作：

1. 進入 **Edit Mode**。
2. 選取要圓形化的一圈 Vertices。
3. 右鍵 → **LoopTools → Circle**。
4. Circle 會重新排列既有頂點的位置，不會單純為了圓形化而增加頂點數。

LoopTools 另外常用的功能包括 **Relax、Space、Flatten、Bridge**，適合角色與一般建模拓樸整理。

## 5. 修改器與法線

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| Displace Modifier | 表面沿法線偏移 | Push Modifier（概念接近） | 並非所有情況都完全等價 |
| `Shift + N` | Recalculate Outside | Unify Normals | Edit Mode |
| Mark Sharp + Smooth by Angle | 控制硬邊 / 平滑 | Smoothing Groups | Blender 4.x 常見工作流 |

## 6. 查看面數

- 3D Viewport → Viewport Overlays → **Statistics**：顯示 Vertices / Edges / Faces / Tris。
- Status Bar 右鍵 → **Scene Statistics**：在底部狀態列顯示統計。
- 遊戲資產效能預算通常優先確認 **Tris**。

## 7. UV 工作流與插件

| 工具 | 定位 | 適合用途 | 注意事項 |
|---|---|---|---|
| TexTools | 免費 UV 工具集 | Texel Density、對齊、Rectify、Checker | Blender 4.x / 4.5 相容性依版本確認 |
| UVPackmaster 3 | 高效 UV Packing | 大量島嶼、UDIM、Packing | 依官方版本確認相容性 |
| Zen UV | 一體化 UV 工作流 | Unwrap、對齊、Quadrify、UV 管理 | 付費插件 |
| RizomUV Bridge | Blender ↔ RizomUV | 高複雜度專業 UV 工作流 | Bridge 與 RizomUV 版本需匹配 |

## 8. Shader / Normal Map

### 反轉 Normal Map 綠色通道

如果需要反轉法線貼圖的 Y / Green 通道：

1. Image Texture → **Separate Color**。
2. R 保留。
3. G 執行 `1 - G`。
4. B 保留。
5. 使用 **Combine Color** 合併 RGB。
6. 輸出接到 **Normal Map** 節點的 Color。

注意：如果使用 Invert Color，要避免把整個 RGB 一起反轉。

## 9. Blender ↔ Unity / Unreal：FBX、單位與座標

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

## 10. 3ds Max → Blender 快速索引

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
| 將一圈頂點整理成圓形 | 安裝 LoopTools → Circle |

## 11. 待補充

- Symmetry ↔ Mirror Modifier
- Relax / Conform / Shrinkwrap 對照
- Pivot / Working Pivot / Transform Orientation
- Edit Poly 常用指令對照
- Unwrap UVW ↔ Blender UV Editor 深入操作
- Blender 4.5 / 4.x 插件相容性
- Unity / Unreal FBX 專案 SOP

---

**持續更新中。** 後續 Blender 問答中，適合長期保留且已確認的內容會整理進這份筆記。