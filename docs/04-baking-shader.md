# 貼圖烘焙與 Shader 技術

[← 返回學習筆記首頁](../README.md) · 3D 技術研究

> [!TIP]
> Base Color、High/Low Normal、Bake 插件、Normal 綠色通道。以 **Blender 4.5** 為主要介面基準，涉及版本差異時以相應版本為準。

### 本頁內容

- 13. Texture Baking 技術與流程
- 13.1 基本烘焙：材質顏色 → 圖片
- 13.2 高模 → 低模 Normal Map
- 13.3 注意事項與排錯
- 13.4 額外 Bake 插件（Blender 4.5 學習參考）
- 14. Shader / Normal Map 技術
- 反轉 Normal Map 綠色通道

---

## 13. Texture Baking 技術與流程

以下以 Blender 4.5 Cycles 示範，技術原則也適用於其他 DCC 的 High/Low 烘焙工作。

## 13.1 基本烘焙：材質顏色 → 圖片

1. 模型先完成 UV 展開，並確認 UV 不會不必要地重疊。
2. Render Properties → Render Engine 設為 **Cycles**。
3. 在模型材質的 Shader Editor 新增 **Image Texture**，按 New 建立目標圖片（例如 2048 × 2048）。**點選 Image Texture 節點，使其成為 Active Node**；無須連接到 BSDF。多材質物件須確保每個相關材質都有正確的目標圖片節點且為 Active。
4. Render Properties → Bake：選 **Diffuse**，只勾 **Color**，取消 **Direct**、**Indirect**，以避免將光照烘焙進顏色。
5. 選取模型按 **Bake**，完成後在 Image Editor → Image → **Save As** 儲存圖片。

若要精確輸出程序化節點計算後的顏色，可以將結果接到 Emission，改用 **Emit** 烘焙；要注意色彩空間及輸出設定。

## 13.2 高模 → 低模 Normal Map

1. 高模與低模正確對位；低模先展好 UV。
2. 在低模材質中新增並選取目標 **Image Texture** 節點（Normal 圖片建議用 Non-Color 解讀）。
3. Bake Type 設為 **Normal**，Space 設 **Tangent**；勾選 **Selected to Active**。
4. Object Mode **先選高模，最後選低模**，確保低模為 Active Object。
5. 調整 **Max Ray Distance**，必要時使用 Cage 避免投射錯誤，再按 **Bake**。
6. 檢查 UV 接縫、法線方向與投射結果，並儲存圖片。

## 13.3 注意事項與排錯

- 烘焙結果空白：確認使用 Cycles、目標 Image Texture 節點為 Active，並檢查 UV。
- 黑邊／接縫：檢查 UV Island 間距與 **Margin**。
- Normal 細節錯亂：檢查高低模對位、Selected to Active、Ray Distance、Cage。
- Blender 通常使用 OpenGL（Y+）切線空間 Normal；若目標引擎或專案使用 DirectX（Y−）慣例，須確認是否反轉綠色通道。
- Bake 完成後仍須 **Image → Save As**，避免只留在暫存圖片中。

## 13.4 額外 Bake 插件（Blender 4.5 學習參考）

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


---

## 14. Shader / Normal Map 技術

## 反轉 Normal Map 綠色通道

如果需要反轉法線貼圖的 Y / Green 通道：

1. Image Texture → **Separate Color**。
2. R 保留。
3. G 執行 `1 - G`。
4. B 保留。
5. 使用 **Combine Color** 合併 RGB。
6. 輸出接到 **Normal Map** 節點的 Color。

注意：如果使用 Invert Color，要避免把整個 RGB 一起反轉。

---

**其他章節：** [01 / 基礎操作](01-blender-basics.md) · [02 / UV 與插件](02-uv-and-tools.md) · [03 / 法線技術](03-normals-hard-surface.md) · [04 / 貼圖與 Shader](04-baking-shader.md) · [05 / 遊戲引擎](05-engine-fbx.md)

[↑ 返回首頁](../README.md)
