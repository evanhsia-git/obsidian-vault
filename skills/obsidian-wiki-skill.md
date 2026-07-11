---
title: "Obsidian Wiki 技能 v3.0"
description: "Obsidian Wiki 導航技能，強制前置導航 + 安全規則 + 三處同步寫入"
summary: "工作流程核心技能 v3.0，每次對話必執行"
type: concept
status: active
tags: [agent, workflow, security]
created: 2026-06-21
updated: 2026-06-25
---

# obsidian-wiki 技能 v3.0

## 🔴 安全規則（不可覆蓋）

1. 任務連續失敗 ≥ 3 次 → 立即停止
2. 未經批准不得刪除、移動、重新命名檔案
3. 無法判斷時停止並請求人工確認（[HOLD]）
4. 資料安全優先於任務完成

## ⚡ 強制前置導航（每次對話必執行）

跳過條件：用戶明確說「跳過導航」

四步導航：
1. `read_file("SCHEMA.md")` — 對齊規範
2. `read_file("policy.md")` — 規則路由
3. `read_file("index.md")` — 定位目標
4. `read_file("log.md", offset=-30)` — 理解近期變更

未完成導航前不處理任何請求。

## 📋 核心操作

### 知識寫入（Ingest）
1. 建立頁面至 `concepts/` 或 `entities/`
2. 更新 `index.md`
3. 追加 `log.md`

### 新建技能：三處同步
| 位置 | 路徑 | 內容 |
|------|------|------|
| Skill 檔案 | `~/.hermes/skills/<category>/<name>/SKILL.md` | 執行規範 |
| Obsidian Wiki | `concepts/<name>.md` + index + log | 人類可讀 + RAG |
| Hermes Memory | — | 僅精簡規則 |

## ⚡ 執行效率規則
- 純文字推理 → 禁止 Python
- 允許 Python → 計算、CSV/Excel/JSON 解析、格式轉換
- 寫入後 → 立即 ls/wc 驗證
- Telegram → 僅用 telegram-message-file-sender

## ⚠️ 常見陷阱
- 跳過導航
- 索引漂移（新頁未加入 index.md）
- LaTeX 洩漏
- 免費模型假裝完成

## 教訓
2026-06-21：未導航直接執行任務 → 安全規範未載入 → 操作失控 → 檔案遺失。

- [[openrouter-free-models]]
## 相關頁面
- 安全檔案操作規範