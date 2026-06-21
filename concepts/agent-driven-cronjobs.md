---
title: Agent 驅動任務 (LLM-driven Cronjob)
description: Agent 驅動任務 (LLM-driven Cronjob) — 概念說明頁面
summary: Agent 驅動任務 (LLM-driven Cronjob)
type: concept
status: active
priority: P2
tags: [hermes, concept, agents, cronjob]
aliases: []
created: 2026-06-12
updated: 2026-06-12
date: 2026-06-12
publish: true
draft: false
related:
source:
due:
review:
---

# 🤖 Agent 驅動任務 (LLM-driven Cronjob)

在 Hermes Agent 系統中，自動化任務主要分為三種類型。**Agent 驅動任務** 是其中最具靈活性、但也最具成本（Token 使用量較高）的一種模式。

## 🎯 定義

**Agent 驅動任務** 是一種透過 Cronjob 排程，在指定時間啟動一個完整的 AI Agent 實例，並交由其執行「包含推理邏輯」的複雜指令。與傳統腳本不同，它不依賴預寫好的程式碼路徑，而是依賴 **Prompt (指令)** 來驅動 Agent 使用工具（如 `terminal`, `web_search`, `read_file`）來達成目標。

---

## 🔄 三種自動化模式對比

| 特性 | 傳統 Script (Python/Bash) | 既有 Skill (Modular Tool) | Agent 驅動任務 (LLM-driven) |
| :--- | :--- | :--- | :--- |
| **核心驅動力** | 寫死的程式碼邏輯 | 模組化的工具函數 | **AI 的推理與理解力** |
| **靈活性** | 極低 (只能做預定動作) | 中 (可組合不同的 Skill) | **極高 (可處理非結構化資訊)** |
| **成本 (Token)** | 極低 (幾乎為 0) | 低 (僅工具呼叫成本) | **高 (需進行完整推理與生成)** |
| **錯誤處理** | 依賴 Try-Catch 邏輯 | 依賴工具回傳值 | **具備自主糾錯與嘗試能力** |
| **最佳場景** | 資料搬移、簡單備份、固定報表 | 抓取特定 API、執行標準化分析 | **摘要分析、異常偵測、跨來源彙整** |

---

## 📖 使用說明 (How to Design)

要設計一個成功的 Agent 驅動任務，Prompt 的品質至關重要。一個高品質的任務 Prompt 應包含以下要素：

1. **明確的目標 (Goal)**：告訴 Agent 最終要產出的結果是什麼（例如：一份 Markdown 報告、一個 SQLite 紀錄）。
2. **資料來源與範圍 (Context/Scope)**：指定 Agent 需要檢閱哪些檔案或搜尋哪些範圍（例如：`read log.md`、`grep yesterday's date`）。
3. **步驟指南 (Workflow)**：雖然 Agent 會自行推理，但給予初步的步驟建議能大幅提升成功率。
4. **輸出格式規範 (Output Format)**：嚴格定義輸出格式（例如：使用特定的 Header、禁止使用某些符號、必須包含特定欄位）。

### 💡 撰寫範例 (Bad vs Good)

❌ **錯誤範例 (過於模糊)**：
> 「幫我看看昨天的日誌並做個報告。」
> *(Agent 可能會跑出冗長的對話、格式混亂、或不知道要寫到哪裡)*

✅ **正確範例 (結構化指令)**：
> 「請執行以下任務：
> 1. 讀取 `/root/Documents/Obsidian Vault/log.md`。
> 2. 使用 `grep` 篩選出包含日期 `$(date -d 'yesterday' +'%Y-%m-%d')` 的所有行。
> 3. 將結果整理成一份『每日任務執行報告』。
> 4. 報告格式要求：
>    - 第一行：`**任務報告 - [日期]**`
>    - 第二行：成功/失敗統計
>    - 第三行：各項任務的摘要說明
> 5. 最後將報告直接輸出於回應中。」

---

## 🚀 應用範例 (Use Cases)

### 1. 跨來源資訊彙整 (Intelligence Synthesis)
* **場景**：每天早上抓取技術新聞、股市新聞與內部 Log，並將三者結合，分析是否有任何外部事件影響了內部的技術變動。
* **價值**：傳統腳本無法理解「新聞內容」與「Log 變動」之間的語意關聯，但 Agent 可以。

### 2. 智慧型健康檢查 (Smart System Audit)
* **場景**：定期掃描 Obsidian Vault，不只是檢查語法錯誤，還要檢查「知識是否過時」或「兩個概念之間是否存在邏輯矛盾」。
* **價值**：能提供具備「洞察力」的維護建議，而不僅僅是錯誤清單。

### 3. 自適應回報系統 (Adaptive Reporting)
* **場景**：根據當天任務的複雜度，自動決定報告的長度與詳細程度。
* **價值**：任務簡單時給出精簡摘要，任務重大時自動擴充成深入分析。

---

## ⚠️ 設計原則與警告

* **成本意識**：若任務可以透過單純的 Python 腳本完成，**絕對不要**使用 Agent 驅動模式。
* **防範「假裝完成」**：對於高成本任務，建議在 Prompt 中加入「驗證步驟」（例如：要求 Agent 在完成後執行 `ls` 或 `wc -l` 來確認檔案已寫入）。
* **防範資訊爆炸**：確保 Agent 知道何時該「保持沉默 (Silent)」，避免在無事可做時噴出大量的冗餘資訊。

---
## 相關節點
- [[hermes-workflow]]
- [[cron-jobs]]