# UV、插件與 Bake 功能對照

[← 返回學習筆記首頁](../README.md) · 功能對照

> [!TIP]
> UV 展開、Sharp → Seam、RizomUV Bridge、RTT 與 Cycles Bake。以 **Blender 4.5** 為主要介面基準，涉及版本差異時以相應版本為準。

### 本頁內容

- 8. UV 功能對照與相關插件
- 8.1 依平滑組／硬邊自動展 UV（3ds Max 對照）
- 8.2 Sharp Edge → Seam → Unwrap 的插件選項（Blender 4.5）
- 8.3 RizomUV：利用硬邊資訊進行 Auto Seams 與 Unfold
- 9. 3ds Max → Blender 快速索引
- 10. Bake to Texture ↔ Blender Cycles Bake
- 10.1 功能與輸出元素對照

---

## 8. UV 功能對照與相關插件

| 工具 | 定位 | 適合用途 | 注意事項 |
|---|---|---|---|
| TexTools | 免費 UV 工具集 | Texel Density、對齊、Rectify、Checker | Blender 4.x / 4.5 相容性依版本確認 |
| UVPackmaster 3 | 高效 UV Packing | 大量島嶼、UDIM、Packing | 依官方版本確認相容性 |
| Zen UV | 一體化 UV 工作流 | Unwrap、對齊、Quadrify、UV 管理 | 付費插件 |
| RizomUV Bridge | Blender ↔ RizomUV | 高複雜度專業 UV 工作流 | Bridge 與 RizomUV 版本需匹配 |


## 8.1 依平滑組／硬邊自動展 UV（3ds Max 對照）

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

## 8.2 Sharp Edge → Seam → Unwrap 的插件選項（Blender 4.5）

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

## 8.3 RizomUV：利用硬邊資訊進行 Auto Seams 與 Unfold

**RizomUV 可以處理這個需求，先前列舉 UV 插件時不應忽略它。** RizomUV 的 **Auto Seams → Sharpest Angles** 適合硬表面模型，可分析幾何角度、Quad 拓樸及 User Vertex Normals 等資訊，自動決定接縫並 Unfold／Pack。RizomUV 更新紀錄亦提到 Auto Seams 的法線來源設定。

**重要區分**：
- 幾何夾角、Blender **Mark Sharp** 標記和傳輸後的 FBX 使用者法線不完全等同。直接送進 RizomUV 的 Auto Seams **不保證逐條採用 Blender 已標記 Sharp Edge**。
- 若要求嚴格沿用 Blender 的 Sharp，較可靠的方式是先在 Blender 把 Sharp 轉為 **Seam**（見 8.1），再以支援 Seam 傳送的 Bridge 進行 Unfold／Pack，或使用支援 Sharp Selection Sync 的 Bridge。

**橋接選擇**：
- [RizomUV Bridge（Superhive）](https://superhivemarket.com/products/rizom-uv-bridge/docs)：支援自動切割、Unfold、Pack、回傳 UV 與傳送 Blender Seam（要檢查 Auto Seams／Cut by My Seams 設定）；需搭配獨立 RizomUV。
- [B2RUVL](https://tituslvr.github.io/B2RUVL/)：文件列出 Sharp／Seam 邊選取同步、Sharp_Edges Lua 腳本；**要求 Blender 5.0+（Windows）**，不適合作為 Blender 4.5 直接安裝的建議。

**Max 對照**：3ds Max 的「By Smoothing Groups」是直接依群組邊界切 UV；RizomUV 的 Sharpest Angles 是自動分析法線／幾何資訊，而非完全同義。若目的為準確沿用 Blender Mark Sharp，採 **Sharp → Seam → Bridge 到 RizomUV → Unfold → Pack → 回傳 Blender**。

參考：[RizomUV 官方算法](https://www.rizomuv.com/c-library/)、[RizomUV 更新紀錄](https://www.rizomuv.com/changelog/)、[RizomUV Bridge 文件](https://superhivemarket.com/products/rizom-uv-bridge/docs)。


---

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


---

## 10. Bake to Texture ↔ Blender Cycles Bake

## 10.1 功能與輸出元素對照

3ds Max **本身就能將材質／程序化紋理烘焙為圖片**，不需要為了 Base Color 烘焙而必須換到 Blender。新版 3ds Max 可從 **Rendering → Bake to Texture** 開始；舊版常使用 **Render To Texture（RTT）**。實際 Bake Elements 的名稱與可用功能依 3ds Max 版本、材質及渲染器而異。

| 目標 | 3ds Max | Blender Cycles |
|---|---|---|
| 材質顏色 → Base Color | Bake to Texture 的 Albedo／Base Color 類輸出元素（視版本／渲染器而定） | Diffuse：只勾 Color，關閉 Direct／Indirect |
| 程序化材質 → 圖片 | Bake to Texture，確認節點與渲染器支援 | 將目標顏色接到 Emission，再使用 Emit Bake |
| 高模 → 低模 Normal | Projection／Bake to Texture | Normal + Selected to Active；依需求使用 Cage |
| AO | 對應 AO 烘焙元素（依渲染器而定） | AO Bake |
| 多材質 → 單張貼圖 | 先規劃共用 UV 與輸出目標，再 Bake | 共用 UV／目標 Image Texture，逐材質設定接收節點 |

**Material Conversion 不等於 Texture Baking**：將 V-Ray／Arnold 等材質轉成其他材質類型，並不代表已輸出 Base Color 圖片。烘焙純色時須排除光照、陰影、AO、反射等非 Base Color 資訊。第三方渲染器的特殊節點未必能被所有 Bake Element 正確輸出。

---

**其他章節：** [01 / 基礎操作](01-blender-basics.md) · [02 / UV 與插件](02-uv-and-tools.md) · [03 / 法線技術](03-normals-hard-surface.md) · [04 / 貼圖與 Shader](04-baking-shader.md) · [05 / 遊戲引擎](05-engine-fbx.md)

[↑ 返回首頁](../README.md)
