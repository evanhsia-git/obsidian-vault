---
title: "Awesome DESIGN.md"
description: "VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式，供 AI Agent 生成高品質 UI"
summary: "Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合，讓 AI 生成風格一致的 UI"
type: concept
status: active
tags: [ai]
created: 2026-06-24
updated: 2026-06-24
---

# Awesome DESIGN.md

[原始連結](https://github.com/VoltAgent/awesome-design-md)

## 核心概念

**DESIGN.md** 是 Google Stitch 引入的純文字設計系統格式：

- 讓 AI Agent 理解並複製專案視覺風格
- 不需 Figma 匯出或 JSON schema
- 直接放入專案根目錄，指示 AI「按照這個設計建頁面」即可
- [官方規範](https://stitch.withgoogle.com/docs/design-md/specification/)

## 架構層級

| 檔案 | 用途 |
|---|---|
| `AGENTS.md` | coding agents 如何建構專案 |
| `DESIGN.md` | design agents 如何感受專案外觀 |

## DESIGN.md 九大區塊

| # | 欄位 | 捕獲內容 |
|---|---|---|
| 1 | 視覺主題與氛圍 | 情緒、密度、設計哲學 |
| 2 | 色板與角色 | 語意色名 + hex code + 功能 |
| 3 | 字體規則 | font family + 層級表格 |
| 4 | 元件樣式 | buttons/cards/inputs/states |
| 5 | 佈局原則 | 間距/格線/留白 |
| 6 | 深度與高程 | shadow/表面層級 |
| 7 | 可做與不可做 | design guardrails |
| 8 | 響應式行為 | breakpoints/觸控/收合 |
| 9 | Agent 提示指南 | 快速色碼參考 + 即用提示 |

## 附加資產

- `preview.html` — 色碼、字體、按鈕、卡片視覺目錄（淺色）
- `preview-dark.html` — 暗色模式版本

## 分類（10 個領域）

- AI & LLM 平台
- 開發者工具/IDE
- 後端/資料庫/DevOps
- 生產力/SaaS
- 設計/創意工具
- 金融科技/加密
- 電商
- 媒體/消費科技
- 汽車
- 復古網頁

## 使用方式

1. 從集合中選擇符合美學的 `DESIGN.md`
2. 放到專案根目錄
3. 對 AI 說：「按照這個 DESIGN.md 讓我看看頁面」

## 價值

- 不需要 Figma access 也能生成風格一致的專業 UI
- 適用於快速 prototyping、風格遷移、樣式標準化
- MIT 開源，所有檔案都是公開可見的 CSS 值

##  REFERENCES

- [Google Stitch DESIGN.md 規範](https://stitch.withgoogle.com/docs/design-md/specification/)
- [awesome-design-md GitHub](https://github.com/VoltAgent/awesome-design-md)
- [Google Stitch](https://stitch.withgoogle.com/)

---

## GitHub Resources

---
title: "Awesome GitHub 列表"
description: "Awesome GitHub 列表 — 實體資料頁面"
summary: "Awesome GitHub 列表"
type: entity
status: active
tags: [hermes]
created: 2026-06-06
updated: 2026-06-06
---

# Awesome GitHub 列表

這些 GitHub Repository 彙整了全球開發者貢獻的 Skills 與 MCP Servers，是尋寶的最佳地點。

## 1. 資源庫
- **[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)**
  - 類型：官方/社區 MCP 伺服器
  - 亮點：包含 Filesystem (讀寫檔案)、GitHub (管理代碼)、Google Drive 等核心功能的官方實作，穩定性最高。
- **[VoltAgent/awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills)**
  - 類型：Agent Skills 集合
  - 亮點：收錄超過 150 個 Agent Skills，涵蓋程式碼審查、創意寫作等多個領域，支援 Claude Code 與 Cursor 等工具。
- **[travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills)**
  - 類型：綜合資源
  - 亮點：專注於自定義工作流與 Claude Code 的整合，適合進階使用者。

## 2. MCP 註冊表與市集
想要像安裝 Chrome Extension 一樣簡單地安裝 MCP 嗎？

- **[Glama - MCP Registry](https://glama.ai/)**
  - 提供可搜尋的 MCP 伺服器列表，界面友善，能快速找到如 Web Search (Brave)、Spotify 控制等有趣工具。
- **[MCP Market](https://mcp.market/)**
  - 精選了高品質的 MCP 整合，例如 Firecrawl (將網站轉為 LLM 易讀格式) 和 Neon (無伺服器 Postgres 資料庫)。

相關頁面：model-error-messages

相關頁面：hermes-memory-system

相關頁面：python-in-skill-implementation

相關頁面：hermes-agent-expansion-guide

相關頁面：hermes-hierarchy-architecture

相關頁面：corporate-collaboration-model

相關頁面：[[vps-config]]

## 相關節點
- [[index]]

## GitHub Resources

