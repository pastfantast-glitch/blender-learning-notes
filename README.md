# 3D 美術學習筆記

**Blender 操作對照 × 遊戲美術技術研究**

> [!NOTE]
> 這份筆記分成兩條閱讀路線：**學習 Blender 對應功能**，以及**理解不限定軟體的 3D 製作技術**。各章節已拆成獨立頁面，不再全部擠在同一份長文裡。

---

## 🟦 第一篇｜Blender 學習筆記

**3ds Max → Blender 轉換指南**

以熟悉的 3ds Max 工作習慣為起點，記錄 Blender 的對應功能、操作位置、快捷鍵與差異。

| 章節 | 內容重點 | 閱讀 |
|:--|:--|:--|
| **01 · Blender 操作與建模對照** | 視圖、Transform、Reset XForm、拓樸、LoopTools、Smoothing Groups | **[開啟章節 →](docs/01-blender-basics.md)** |
| **02 · UV、插件與 Bake 對照** | UV 展開、Sharp → Seam、RizomUV、Zen UV、Max RTT ↔ Blender Bake | **[開啟章節 →](docs/02-uv-and-tools.md)** |

> [!TIP]
> **從 Max 尋找 Blender 的相同功能？** 建議先讀第一篇。內建插件與官方 Extensions 收錄於第 01 章的專區，第三方 UV 工具則依功能整理。

---

## 🟩 第二篇｜3D 技術研究與製作流程

**不侷限 Blender 或 3ds Max**。著重製作原理、何時使用技術、工作流程與引擎呈現。

| 章節 | 內容重點 | 閱讀 |
|:--|:--|:--|
| **03 · 法線與硬表面技術** | Normal Transfer、Weighted Normal、單段 Bevel、High／Low 烘焙前的法線設定 | **[開啟章節 →](docs/03-normals-hard-surface.md)** |
| **04 · 貼圖烘焙與 Shader** | Base Color、Normal Map、Bake 排錯、Bake 插件與綠色通道 | **[開啟章節 →](docs/04-baking-shader.md)** |
| **05 · Unity／Unreal FBX 工作流程** | 單位、軸向、Root Rotation、匯出與匯入檢查 | **[開啟章節 →](docs/05-engine-fbx.md)** |

> [!IMPORTANT]
> **技術決策與軟體按鈕分開記錄。** 例如「Mark Sharp 在哪裡」屬於第一篇；「單段 Bevel 的低模是否還要切硬邊」屬於第二篇。避免重複，也方便之後擴充 ZBrush、Substance、Unity 或 Unreal 技術。

---

## 📌 待研究與後續補充

- **[功能對照待補充清單 →](docs/02a-feature-todo.md)**：Mirror、Pivot、Edit Poly、UV Editor 與 Export SOP 等主題。
- 已記錄的項目會保留版本、適用條件與注意事項；尚未驗證的設定不寫成通用規則。

---

## 如何使用這份筆記

| 想做什麼？ | 從哪裡開始？ |
|:--|:--|
| 查「Max 這個功能在 Blender 哪裡？」 | [第一篇 · 操作對照](docs/01-blender-basics.md) |
| 查 UV、Seam 與插件 | [第一篇 · UV 與 Bake 對照](docs/02-uv-and-tools.md) |
| 查硬邊、法線傳導、Weighted Normal | [第二篇 · 法線技術](docs/03-normals-hard-surface.md) |
| 查高低模烘焙、Base Color、Shader | [第二篇 · 貼圖與 Shader](docs/04-baking-shader.md) |
| 查 Blender 與 Unity／Unreal 匯出 | [第二篇 · FBX 工作流程](docs/05-engine-fbx.md) |

<sub>筆記以 Blender 4.5 的操作環境為主要參考；介面、插件相容性及遊戲引擎設定可能隨版本改變。GitHub Markdown 不支援穩定的自訂全文字色，因此使用章節色彩標記、原生提示框、表格留白與獨立頁面改善閱讀。</sub>
