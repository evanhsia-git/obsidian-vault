---
status: active
title: "量化投資研究 AI Agent (Quant Python AI Agent)"
description: "量化 Python AI Agent 實作"
summary: "量化投資研究 AI Agent (Quant Python AI Agent)：概述"
created: 2026-06-02
updated: 2026-06-02
type: concept
tags: [tw-stock, agent]
---

# 量化投資研究 AI Agent (Quant Python AI Agent)

## 概述


- stock-data-sources
- stock-analysis-workflow-full

「量化投資研究 AI Agent」是一個專為量化投資領域設計的 AI Agent，其核心功能在於自動化地執行多項與金融市場分析相關的任務。

## 核心功能

- **自動搜尋財經新聞**：能夠從各種來源獲取最新的財經新聞資訊。
- **分析市場情緒**：對新聞內容進行情感分析，評估市場的整體情緒傾向。
- **產生風險評估報告**：根據收集到的資料和情緒分析，自動生成詳盡的風險評估報告。

## 互動介面

該 Agent 透過 CLI (Command Line Interface) 互動介面操作，提供使用者友善的指令輸入方式來啟動和管理其功能。這使得研究人員和開發者可以方便地與 Agent 進行互動，觸發資料收集、分析和報告生成等任務。

## 擴展性

此 Agent 具有良好的擴展性，特別提及「可接 OpenClaw」，意味著它可能設計為可以與其他工具或框架進行整合，以擴展其功能或連接到更廣泛的生態系統。

## 參考連結

- **GitHub 專案**：[aidatatools/quant-python-ai](https://github.com/aidatatools/quant-python-ai)
- **官方網站**：[quantpython.ai](https://quantpython.ai)

## Financial Preferences

---
status: active
description: "用戶針對金融分析的特定偏好與品質標準。"
title: "Financial-Preferences"
summary: "用戶金融分析偏好與品質標準規範"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[openrouter-free-models]]
- 股票自動化配置

# 金融分析偏好規範 (Financial Preferences)

## 1. 報告風格
- **核心模式**：文本主導 → 數據佐證。
- **視覺要求**：
    - 僅在深度分析模式下使用局部量化圖表。
    - 必須移除所有 Emoji 符號，改用專業文本標記（如 `[高風險]`）。
    - 優先使用繁體中文，僅在專業術語 (DCF, WACC, SGE) 保留英文。
- **交付方式**：提到「傳送給我」時，優先使用 Telegram 傳送檔案而非直接顯示。

## 2. 數據與工具偏好
- **優先來源**：TWSE OpenAPI → TPEX → FinMind → OpenBB。
- **資料庫傾向**：傾向於將分析結果結構化並存入 SQLite / DuckDB，以支持後續 RAG 引用。
- **更新邏輯**：執行任務後發現新方法或 Pitfall 時，必須自動更新對應 skill。

## 3. 關鍵指標關注點
- **台股**：極其關注 ROE、淨利率、負債比。
- **美股**：關注 AI 基礎設施資本支出 (CapEx) 與 Azure/Cloud 增速。
- **宏觀**：關注聯準會利率方向與 CPI 通膨數據。

## Arena Leaderboard

---
status: active
title: "Arena AI 開源模型排行榜 (Top 50)"
summary: "Arena AI 開源模型排行榜 (Top 50)：頂尖開源模型 (前 10 名)"
created: 2026-06-02
updated: 2026-06-02
type: concept
tags: [ai, performance, linux]
---

# Arena AI 開源模型排行榜 (前 50 名摘要)

本頁面彙整來自 [LMSYS Arena](https://arena.ai/leaderboard/text?license=open-source) 的開源模型排名數據 (截至 2026-05-28)。

## 頂尖開源模型 (前 10 名)

| 排名 | 模型名稱 | 得分 | 價格 ($/M 輸入/輸出) | 上下文窗口 |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `glm-5.1` | 1474 | $1.40 / $4.40 | 202.8K |
| 2 | `mimo-v2.5-pro` | 1465 | $0.43 / $0.87 | 1M |
| 3 | `kimi-k2.6` | 1462 | $0.95 / $4 | 262.1K |
| 4 | `deepseek-v4-pro-thinking` | 1458 | $0.43 / $0.87 | 1M |
| 5 | `glm-5` | 1457 | $1 / $3.20 | 202.8K |
| 6 | `deepseek-v4-pro` | 1454 | $0.43 / $0.87 | 1M |
| 7 | `gemma-4-31b` | 1452 | $0.14 / $0.40 | 262.1K |
| 8 | `kimi-k2.5-thinking` | 1449 | $0.60 / $3 | N/A |
| 9 | `qwen3.5-397b-a17b` | 1445 | $0.39 / $2.34 | 262.1K |
| 10 | `glm-4.7` | 1443 | $0.40 / $1.75 | 202.8K |

*(註：原始排行榜包含超過 360 個模型，此處僅列出 Arena 開源分類下的前 10 名)*

## 摘要與觀察
- **效能趨勢**：`glm-5.1` 與 `mimo-v2.5-pro` 在目前開源領域佔據統治地位。
- **性價比選擇**：`gemma-4-31b` 以極低的價格提供極高的表現，是開發自動化任務（如爬蟲與新聞摘要）的極佳選擇。
- **長文本支援**：`mimo-v2.5-pro` 與 `deepseek-v4-pro` 提供高達 1M 的上下文窗口，適合處理超長文件。

---
*來源：[Arena AI Leaderboard](https://arena.ai/leaderboard/text?license=open-source)*

---
## 相關節點
- openrouter-cheapest-models
- model-error-messages

## Financial Preferences

# 金融分析偏好規範 (Financial Preferences)

## 1. 報告風格
- **核心模式**：文本主導 → 數據佐證。
- **視覺要求**：
    - 僅在深度分析模式下使用局部量化圖表。
    - 必須移除所有 Emoji 符號，改用專業文本標記（如 `[高風險]`）。
    - 優先使用繁體中文，僅在專業術語 (DCF, WACC, SGE) 保留英文。
- **交付方式**：提到「傳送給我」時，優先使用 Telegram 傳送檔案而非直接顯示。

## 2. 數據與工具偏好
- **優先來源**：TWSE OpenAPI → TPEX → FinMind → OpenBB。
- **資料庫傾向**：傾向於將分析結果結構化並存入 SQLite / DuckDB，以支持後續 RAG 引用。
- **更新邏輯**：執行任務後發現新方法或 Pitfall 時，必須自動更新對應 skill。

## 3. 關鍵指標關注點
- **台股**：極其關注 ROE、淨利率、負債比。
- **美股**：關注 AI 基礎設施資本支出 (CapEx) 與 Azure/Cloud 增速。
- **宏觀**：關注聯準會利率方向與 CPI 通膨數據。
## Arena Leaderboard


| 排名 | 模型名稱 | 得分 | 價格 ($/M 輸入/輸出) | 上下文窗口 |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `glm-5.1` | 1474 | $1.40 / $4.40 | 202.8K |
| 2 | `mimo-v2.5-pro` | 1465 | $0.43 / $0.87 | 1M |
| 3 | `kimi-k2.6` | 1462 | $0.95 / $4 | 262.1K |
| 4 | `deepseek-v4-pro-thinking` | 1458 | $0.43 / $0.87 | 1M |
| 5 | `glm-5` | 1457 | $1 / $3.20 | 202.8K |
| 6 | `deepseek-v4-pro` | 1454 | $0.43 / $0.87 | 1M |
| 7 | `gemma-4-31b` | 1452 | $0.14 / $0.40 | 262.1K |
| 8 | `kimi-k2.5-thinking` | 1449 | $0.60 / $3 | N/A |
| 9 | `qwen3.5-397b-a17b` | 1445 | $0.39 / $2.34 | 262.1K |
| 10 | `glm-4.7` | 1443 | $0.40 / $1.75 | 202.8K |

*(註：原始排行榜包含超過 360 個模型，此處僅列出 Arena 開源分類下的前 10 名)*

## 摘要與觀察
- **效能趨勢**：`glm-5.1` 與 `mimo-v2.5-pro` 在目前開源領域佔據統治地位。
- **性價比選擇**：`gemma-4-31b` 以極低的價格提供極高的表現，是開發自動化任務（如爬蟲與新聞摘要）的極佳選擇。
- **長文本支援**：`mimo-v2.5-pro` 與 `deepseek-v4-pro` 提供高達 1M 的上下文窗口，適合處理超長文件。

---
*來源：[Arena AI Leaderboard](https://arena.ai/leaderboard/text?license=open-source)*

---
## 相關節點
- openrouter-cheapest-models
- model-error-messages