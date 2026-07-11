---
title: "Hermes Agent 執行策略"
description: "Hermes Agent 執行策略 — 概念說明頁面"
summary: "Hermes Agent 執行策略"
type: concept
status: active
tags: [deploy]
created: 2026-05-31
updated: 2026-05-31
---

## 相關頁面
- [[concepts/hermes-configuration|Hermes 配置]]
- Obsidian Wiki 使用規範


# Hermes Agent 效能優化與任務執行策略

為了達到「加快執行動作」的目標，Hermes Agent 採用階層式資源存取策略，避免重複性工作與不必要的延遲。

## 資源存取階層 (Hierarchy of Access)

### 1. 記憶庫優先 (快取層)
- **觸發方式**: 系統啟動與每一則訊息自動注入。
- **存取資源**: `MEMORY.md` 與 `USER PROFILE`。
- **用途**: 處理偏好設定（如 PDF 字體、輸出格式、傳送方式）、 recurring 習慣與已建立的穩定規範。這是最快速的執行層。

### 2. 會話紀錄回溯 (歷史層)
- **觸發方式**: 當涉及過去任務流程或邏輯確認時。
- **存取工具**: `session_search`。
- **用途**: 利用 `bookend`（會話頭尾）快速重組過去任務的 goal $\rightarrow$ resolution，避免閱讀全文。

### 3. 知識庫層 (深度查閱層)
- **觸發方式**: 當任務涉及「知識檢索」、「數據篩選」或「關聯分析」時。
- **存取資源**: `Obsidian Vault` (Wiki) 與 `SQLite 資料庫`。
- **行為規範**: Agent 不會自動掃描所有筆記以避免延遲；若您下達指令時提及「查閱 wiki」、「ob 內容」、「SQLite 記錄」，Agent 將優先調用這些資源。

## 給使用者的建議：如何讓 Agent 更快？
若希望 Agent 主動調用特定知識庫資源，請在指令中使用以下關鍵詞：
- **「查閱 ob / wiki...」**: 強制 Agent 優先同步最新的知識框架。
- **「依據 SQLite 儲存的...」**: 強制 Agent 調用資料庫內的結構化數據。

---
*註: 此內容為 Agent 對於任務執行邏輯的自我檢視與說明。*

相關頁面：[[ivan-notes/hermes/environment-keys|Environment Keys]]

相關頁面：news-push-log


## 相關節點
- [[index]]

## Expansion Guide
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
- 統一股票資料欄位 Schema
- [[投資大師選股策略-投資策略investment-strategy|股市分析系統建置指南]]

## Expansion Guide

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
- 統一股票資料欄位 Schema
- [[投資大師選股策略-投資策略investment-strategy|股市分析系統建置指南]]
