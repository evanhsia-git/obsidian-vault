---
status: active
title: "Hermes Agent 擴充與工具開發指南"
summary: "Hermes Agent 擴充與工具開發指南：1. 核心搜尋策略"
created: 2026-06-04
updated: 2026-06-21
type: concept
tags: [hermes, agent, deploy, integration]
---

# Hermes Agent 擴充與工具開發指南

本頁面記錄如何為 Hermes Agent 尋找、評估並整合外部工具與開源資源。

## 1. 核心搜尋策略
Hermes Agent 基於 MCP 與自主代理架構，尋找資源時應優先關注「生態系統級」分類，而非通用應用程式標籤。

### 關鍵 GitHub Topics (標籤)
*   ****：**最重要的標籤**。此類專案可透過  技能直接接入，變為 Agent 的原生工具。
*   ****：搜尋具備獨立邏輯的 Agent 系統，用於參考工具鏈 (Toolchain) 設計。
*   ****：尋找能處理複雜多步驟任務的程式碼架構。
*   ****：Hermes Agent 依賴 CLI 互動，此類工具可直接封裝為 Skill 並透過  工具執行。

## 2. 工具整合路徑
當發現合適的開源工具時，應依據以下優先級進行整合：

1.  **MCP Server 整合 (最優)**：若該工具有 MCP 支援，直接在  中註冊。
2.  **CLI 封裝**：若該工具為 CLI，將其放入 ，並透過  封裝為 Skill。
3.  **API 腳本**：若僅有 SDK，編寫 Python 腳本並透過  或  調用。

## 3. 搜尋關鍵字模式
*   `[工具名稱] + mcp`：例如 "google-drive mcp"
*   `[工具名稱] + cli`：例如 "jira-cli"
*   `[使用場景] + workflow-automation`

---
## 相關資源
- [[concepts/unified-stock-schema|統一股票資料欄位 Schema]]
- [[concepts/stock-analysis-system-guide|股市分析系統建置指南]]
