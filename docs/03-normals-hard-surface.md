# 法線與硬表面技術

[← 返回學習筆記首頁](../README.md) · 3D 技術研究

> [!TIP]
> Normal Transfer、Weighted Normal、低模倒角與 Normal Bake。以 **Blender 4.5** 為主要介面基準，涉及版本差異時以相應版本為準。

### 本頁內容

- 12. 法線與硬表面技術
- 12.1 法線傳導（Normal Transfer / Data Transfer）
- 12.2 法線傳導與 High Poly → Low Poly Normal Bake 的關係
- 12.3 硬表面低模：單段 Bevel（原一條硬邊 → 兩條邊 + 一個斜面）是否還要切 Sharp？
- 12.4 Weighted Normal 是否需要輸出到 Unity／Unreal？

---

## 12. 法線與硬表面技術

## 12.1 法線傳導（Normal Transfer / Data Transfer）

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

## 12.2 法線傳導與 High Poly → Low Poly Normal Bake 的關係

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

## 12.3 硬表面低模：單段 Bevel（原一條硬邊 → 兩條邊 + 一個斜面）是否還要切 Sharp？

**先釐清幾何**：原本 90° 的單條硬邊，執行 **Bevel Segments = 1** 後產生一個斜面、兩條交界邊；**不是 Segments = 2** 的兩段倒角。這個斜面具有實際表面朝向，可提供實體高光，但輪廓仍只有一段直線倒角，並非圓弧。

**結論：不一定需要切平滑組／Mark Sharp。** 優先測試 **Shade Smooth**，若大平面法線受倒角牽動、產生奇怪漸層，可嘗試 **Weighted Normal**；必要時搭配 Bevel Modifier 的 **Harden Normals / Face Strength**。它們會調整 Shading，但不會增加幾何弧度。

- 想要柔和的高光過渡：通常保留 Smooth 並檢查 Weighted Normal 效果。
- 想保留大平面的平直法線，或需要明確折角：可以在斜面兩側 **Mark Sharp**，但高光會更不連續。
- **High → Low Normal Map Bake**：在烘焙前決定法線／Sharp／UV 與必要的 Weighted Normal。硬邊對應處應切 UV Island，減少切線空間接縫問題；反之 UV Seam 不一定要切硬邊。
- 最後以高光、貼圖與 Unity／Unreal 中實際法線表現作判斷。**不是所有單段倒角都一定需要切 Sharp，也不是一定不能切。**

**推薦順序**：Bevel Segments = 1 → Shade Smooth → 檢查大平面與斜面高光 → 視情況 Weighted Normal → 必要時 Mark Sharp + UV Split → Normal Bake。

參考：[Marmoset Baking Tips & Tricks](https://docs.marmoset.co/docs/tips-tricks/)、[Blender Bevel Modifier](https://docs.blender.org/manual/en/4.5/modeling/modifiers/generate/bevel.html)。

## 12.4 Weighted Normal 是否需要輸出到 Unity／Unreal？

**需要保留的是 Weighted Normal 計算後的 Custom Normals，不是 Modifier 本身。** FBX 只承載最終 Mesh／Normals，不會把 Blender 的 Weighted Normal Modifier 原樣帶到遊戲引擎。

**Blender 4.5 FBX 輸出建議**：
1. 保留 Blender 檔案中的 Weighted Normal Modifier，以便日後非破壞性調整。
2. File → Export → FBX；Geometry → **Apply Modifiers** 開啟，輸出修改器評估後的模型。
3. Geometry → **Smoothing: Normals Only**，適用於支援自訂分離法線的匯入器。
4. 將匯出 FBX 重新匯入測試，確認大平面、Bevel 高光以及自訂法線結果。

**Unity**：FBX Model Import Settings → **Normals: Import**，避免引擎用 Calculate 覆蓋 Blender 的法線；**Tangents: Calculate Mikktspace** 可作為一般切線空間 Normal Map 工作流程的起點，但仍需與烘焙端的切線空間一致。Unreal 也應選擇保留／匯入自訂法線的設定，實際名稱依引擎版本而異。

**Normal Bake 一致性**：在 High → Low Bake 之前定案 Weighted Normal、Sharp、UV 與三角化；最後匯入引擎的低模法線應盡量和 Bake 時相同。若事後關閉 Weighted Normal 或在引擎重新計算法線，Normal Map 可能出現高光、陰影、接縫異常。

參考：[Blender 4.5 FBX Manual](https://docs.blender.org/manual/en/4.5/addons/import_export/scene_fbx.html)、[Unity Model Import Settings](https://docs.unity.com/en-us/engine/7000.0/manual/assets-and-media/asset-types/models/importing/fbximporter-model)。

---

**其他章節：** [01 / 基礎操作](01-blender-basics.md) · [02 / UV 與插件](02-uv-and-tools.md) · [03 / 法線技術](03-normals-hard-surface.md) · [04 / 貼圖與 Shader](04-baking-shader.md) · [05 / 遊戲引擎](05-engine-fbx.md)

[↑ 返回首頁](../README.md)
