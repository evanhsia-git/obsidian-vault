---
title: Obsidian Wiki 使用規範
summary: Obsidian Wiki 使用規範：相關頁面
created: 2026-06-01
updated: 2026-06-01
type: concept
tags: [obsidian, rules, wiki, conventions]
---

## 相關頁面
- [[SCHEMA|Wiki 規範]]
- [[index|Wiki 索引]]


# Obsidian Wiki 使用規範

## 語言規範

- **所有對話與問答一律使用繁體中文（Traditional Chinese）**
- 禁止使用簡體中文
- 專有名詞可保留英文原文，但敘事主體為繁體中文

## 輸出格式規範

- **禁止使用 $\LaTeX$ 數學符號**：嚴禁在輸出中使用 `$→$`, `$⇒$`, `$\text{...}$` 等格式，因為 Telegram 界面無法渲染 $\LaTeX$。
- **強制使用 Unicode**：所有方向箭頭、數學符號必須替換為對應的純文字 Unicode 符號（例如：→, ⇒, ≤, ≥, ±, ×）。
- **目的**：確保所有訊息在 Telegram 設備上都能正確、簡潔地顯示。

## Wiki 操作規範

### ⛔ 結構變更前必須審核
任何編輯、移動、刪除 Wiki 頁面或目錄前，必須遵守：

1. **告知**：向 Ivan 說明要執行什麼操作
2. **方案**：提供完整的執行方案（影響範圍、預計結果）
3. **審核**：等待 Ivan 審核通過
4. **執行**：核可後才開始
5. **記錄**：完成後更新 `log.md`

### 頁面慣例
- 檔案命名：小寫、連字號、無空格
- 每頁必須包含 YAML frontmatter
- 修改頁面時務必更新 `updated` 日期
- 新頁面必須加入 `index.md`

---
*由 Hermes Agent 於 2026-06-01 建立，根據 Ivan 指示記錄語言與操作規範。*
