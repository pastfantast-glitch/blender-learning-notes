# Blender 操作與建模對照

[← 返回學習筆記首頁](../README.md) · 3ds Max → Blender

> [!TIP]
> 從視圖與座標、Reset XForm、拓樸、LoopTools 到平滑組。以 **Blender 4.5** 為主要介面基準，涉及版本差異時以相應版本為準。

### 本頁內容

- 1. 視圖、狀態與座標
- 2. 軸心、Reset XForm 與物件管理
- 3. Edge → Curve
- 4. 網格編輯與拓樸
- 5. 內建插件 / 官方 Extensions
- 5.1 LoopTools
- 安裝／啟用（Blender 4.5）
- Circle：將選取頂點排列成圓形
- 6. 修改器與法線
- 6.1 3ds Max Smoothing Groups ↔ Blender 4.5 Sharp Edges
- 7. 查看面數

---

## 1. 視圖、狀態與座標

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| `G → Z → Z` | 沿物件 Local Z 軸移動 | Local Transform | 雙擊軸向可切到物件自身軸向 |
| `N` | 開啟 3D Viewport 側邊欄 | Transform / Utility 面板 | 可查看 Location / Rotation / Scale |
| `T` | 顯示 / 隱藏左側工具列 | 工具列概念 | 部分建模工具可用滑鼠操作 |


---

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


---

## 3. Edge → Curve

對應 Max 的 **Create Shape from Selection**：

1. Edit Mode 選取 Edge。
2. `Shift + D` 複製，右鍵取消位移。
3. `P → Selection` 分離成獨立 Mesh。
4. Object Mode → 右鍵 → Convert To → Curve。
5. Curve Data Properties → Geometry → Bevel → Depth 可產生厚度。


---

## 4. 網格編輯與拓樸

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| `Alt + S` | Shrink/Fatten | Push | 沿頂點法線方向收縮 / 膨脹 |
| Mesh → Transform → Shrink/Fatten | UI 選單版本 | Push | 不使用快捷鍵時使用 |
| Options → Correct Face Attributes | 幾何移動時補償 UV / Face Corner 資料 | Preserve UVs | 並非所有拓樸操作都能完全無損保持 UV |
| `G → G` | Edge Slide | Edge Slide | 沿現有拓樸滑動 |
| `Alt + J` | Tris to Quads | Quadrify 類似功能 | 可依 UV / Seam / Sharp 等條件限制合併；不是任意模型皆可無損還原 |


---

## 5. 內建插件 / 官方 Extensions

這一章專門收錄 Blender 內建附加元件、過去隨 Blender 提供的 bundled add-ons，以及目前透過官方 Blender Extensions 平台安裝的工具。

> 規則：只要屬於 Blender 官方隨附／官方 Extensions 體系，就統一放在這一章；第三方商業或外部插件則放在對應主題章節。

## 5.1 LoopTools

LoopTools 是 Blender 常用的 Mesh 建模工具集。它在 **Blender 4.1 以前屬於 bundled add-on**；Blender 4.2 之後改由官方 **Blender Extensions** 平台提供。

## 安裝／啟用（Blender 4.5）

1. 開啟 **Edit → Preferences**。
2. 左側進入 **Get Extensions**。
3. 搜尋 `LoopTools`。
4. 找到 **LoopTools**，按 **Install**。
5. 安裝完成後確認已啟用；若之後停用，可回到 Preferences 的 Extensions / Add-ons 管理頁重新 Enable。

如果無法連到 Extensions 平台，也可以下載 LoopTools 的 `.zip`，再到 Preferences → Get Extensions 右上選單使用 **Install from Disk** 安裝。

## Circle：將選取頂點排列成圓形

**LoopTools → Circle** 可以把選取的一圈頂點重新排列成規則圓形，適合製作孔洞、圓形開口與整理圓環拓樸。

操作：

1. 進入 **Edit Mode**。
2. 選取要圓形化的一圈 Vertices。
3. 右鍵 → **LoopTools → Circle**。
4. Circle 會重新排列既有頂點的位置，不會單純為了圓形化而增加頂點數。

LoopTools 另外常用的功能包括 **Relax、Space、Flatten、Bridge**。


---

## 6. 修改器與法線

| Blender | 功能 | 3ds Max 對照 | 注意事項 |
|---|---|---|---|
| Displace Modifier | 表面沿法線偏移 | Push Modifier（概念接近） | 並非所有情況都完全等價 |
| `Shift + N` | Recalculate Outside | Unify Normals | Edit Mode |
| Mark Sharp + Smooth by Angle | 控制硬邊 / 平滑 | Smoothing Groups | Blender 4.x 常見工作流 |

## 6.1 3ds Max Smoothing Groups ↔ Blender 4.5 Sharp Edges

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


---

## 7. 查看面數

- 3D Viewport → Viewport Overlays → **Statistics**：顯示 Vertices / Edges / Faces / Tris。
- Status Bar 右鍵 → **Scene Statistics**：在底部狀態列顯示統計。
- 遊戲資產效能預算通常優先確認 **Tris**。

---

**其他章節：** [01 / 基礎操作](01-blender-basics.md) · [02 / UV 與插件](02-uv-and-tools.md) · [03 / 法線技術](03-normals-hard-surface.md) · [04 / 貼圖與 Shader](04-baking-shader.md) · [05 / 遊戲引擎](05-engine-fbx.md)

[↑ 返回首頁](../README.md)
