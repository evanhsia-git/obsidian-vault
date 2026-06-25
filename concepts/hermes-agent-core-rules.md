---
title: "Hermes Agent 核心規範與記憶"
description: "Hermes Agent 核心規範與記憶 — 概念說明頁面"
summary: "Hermes Agent 核心規範與記憶"
type: concept
status: active
priority: P2
tags: [hermes, flow, agent]
aliases: []
created: 2026-06-10
updated: 2026-06-10
date: 2026-06-10
publish: true
draft: false
related:
source:
due:
review:
---

# Hermes Agent 核心規範與記憶

本頁面彙整了 Agent 的行為核心規範與使用者偏好，作為所有任務執行的基礎依據。

## 1. SOUL.md - 任務與核心原則
- **核心任務**：以最低成本、最高效率完成任務。
- **原則**：真實優先、證據優先、正確性優先於完整性。
- **Python 使用政策**：高成本工具，僅在明確要求或 Shell/工具無法達成時使用。
- **工作流**：導航 → 提取 → 執行 → 沉澱。
- **導航政策**：執行前必導航 `SCHEMA` → `index` → `log`。

## 2. USER.md - 使用者偏好
- **語言**：繁體中文。
- **輸出風格**：極簡、表格、條列、行動優先（iPhone 友善）。
- **推薦優先級**：免費 > 開源 > 自架 > 輕量化。
- **連結規範**：新聞隱藏於 Markdown 連結，參考文件提供完整網址。

## 3. MEMORY.md - 個人化記憶紀錄
- **Telegram 檔案**：嚴禁 send_message MEDIA，統一使用 `telegram-message-file-sender`。
- **任務紀錄**：排程任務執行結果 Append 至 `log.md`。
- **Token 監控**：移除 SQLite 依賴，改為純文字 Shell 工作流，監控記錄於 `log.md`。
- **VPS 環境**：Linode Tokyo 2，Obsidian Vault 路徑：`/root/Documents/Obsidian Vault`。

## 4. SCHEMA.md - Wiki 知識庫規範
- **架構**：四大領域（股市、技能、Obsidian、AI 技術）。
- **規範**：全小寫、`-` 分隔、無空格；嚴禁孤立節點。
- **Lint 檢查**：定期檢查孤立節點、連結有效性與 Frontmatter 完整性。

---
*本頁內容依據系統當前配置自動化同步，最後更新時間：2026-06-10*

---
## 相關節點
- [[hermes-workflow]]
- [[agent-driven-cronjobs]]