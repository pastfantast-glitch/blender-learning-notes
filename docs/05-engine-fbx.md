# Unity／Unreal：FBX、座標與輸出

[← 返回學習筆記首頁](../README.md) · 3D 技術研究

> [!TIP]
> Blender 與引擎間的尺寸、旋轉、FBX 匯出注意事項。以 **Blender 4.5** 為主要介面基準，涉及版本差異時以相應版本為準。

### 本頁內容

- 15. Unity / Unreal：FBX、單位與座標技術
- Blender 基準
- Unity 0.01 / 90° 問題
- 舊筆記中需要避免當成絕對規則的說法

---

## 15. Unity / Unreal：FBX、單位與座標技術

這一區是舊 Gemini 筆記中最需要謹慎處理的部分。FBX 單位、Blender Exporter、Unity Model Importer、Unreal Importer 與 Axis Conversion 會互相影響，不應把單一設定視為所有專案的固定規則。

## Blender 基準

- 常見工作流：Metric、Unit Scale = `1.0`。
- 輸出前確認 Rotation / Scale，尤其是非均勻縮放。
- 外部 FBX 匯入 Blender 後若有 Empty / Parent，先確認用途再決定是否解除。

## Unity 0.01 / 90° 問題

- 區分 **FBX Importer Scale Factor / Convert Units** 與 **Hierarchy GameObject Transform Scale**。
- 可建立 Blender `1m × 1m × 1m` Cube，與 Unity 內建 Cube 並排驗證世界尺寸。
- 若世界尺寸正確且場景 GameObject Scale = `1,1,1`，不要只因 Importer 內部顯示 `0.01` 就持續修改 Blender。
- 若尺寸正確但 Root Rotation 有 ±90°，再檢查 Axis Conversion、Forward / Up 與引擎 Import 設定。

## 舊筆記中需要避免當成絕對規則的說法

- Apply Transform 一定能讓 Unity / Unreal Root 變成 `0,0,0`。
- Forward / Up 絕對不能修改，否則一定 Double Conversion。
- FBX 是公分，因此 Unity 一定會讓 Transform 變成 `0.01`。
- Unity Transform 不是 `1,1,1` 就一定有問題。

---

**其他章節：** [01 / 基礎操作](01-blender-basics.md) · [02 / UV 與插件](02-uv-and-tools.md) · [03 / 法線技術](03-normals-hard-surface.md) · [04 / 貼圖與 Shader](04-baking-shader.md) · [05 / 遊戲引擎](05-engine-fbx.md)

[↑ 返回首頁](../README.md)
