---
title: "equity-research-text 2.0"
description: "專為 Telegram 優化的高密度文字個股分析報告。採用 v2.0-Interactive 模版，將深度量化分析與快訊格式結合，實現「秒讀」級的資訊"
summary: "專為 Telegram 優化的高密度文字個股分析報告"
type: project
status: active
tags:
  - skills
  - finance
created: 2026-06-28
updated: 2026-06-28
---


---
name: equity-research-text
category: finance
description: 專為 Telegram 優化的高密度文字個股分析報告。採用 v2.0-Interactive 模版，將深度量化分析與快訊格式結合，實現「秒讀」級的資訊獲取。
version: 2.0.0
author: ivanhsia
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [text-only, telegram, high-density, equity-research, interactive]
---

# Equity Research Text Skill v2.0 (Interactive Quick-Report)

## 1. 核心目標
提供一份不需要附件、直接在 Telegram 聊天視窗內完成的「高密度量化快訊」。將複雜的投行分析壓縮至模組化結構中，優先滿足手機端閱讀體驗。

## 2. 強制輸出模版 (Strict Template)
所有執行此技能的報告必須嚴格遵循以下結構：

---
📦 **[TICKER] / [Company Name] 投資研究快訊 v2.0-Interactive**

🔔 **Hermes 等級：[S/A/B/C] │ 封包: [HSP-ID/手動生成]**
**市場狀態：[一句話定義目前市場對該標的的共識]**

■ **最近 3 日熱門新聞：**
- [新聞標題 1] [連結](URL) → [一句話影響分析]
- [新聞標題 2] → [一句話影響分析]
- [新聞標題 3] → [一句話影響分析]

■ **標的概況：**
[約 150-200 字的深度敘事。涵蓋：核心競爭力、目前處於週期的哪個位置、最關鍵的一個矛盾點/賭注。]

================================

📊 **核心數據與相對估值**
• 目前價格：$[Price] ([Change]%) [燈號]
• 本益比 P/E：[Value]x ([vs 同業中位數 %]) [燈號]
• ROE %：[Value]% ([vs 同業中位數 %]) [燈號]
• 股息殖利率：[Value]% [燈號]
• 核心指標 [例如: 自由現金流/營收成長]：[Value] [燈號]

📉 **量化指標快照 (Quant Pulse)**
• RSI(14)：[Value] → [狀態: 超買/超賣/中性]
• MACD：[狀態: 金叉/死叉/橫盤] → [趨勢: 多頭/空頭]
• 最大回撤 MDD：[Value]% → [風險等級: 低/中/高]

⚖️ **最終判定 (Verdict)**
[一句話結論：買入/持有/賣出] → [建議持有週期] → [核心觸發條件]
---

## 3. 數據判定規範 (Data Logic)
- **燈號定義**：
    - `🟢`：優於同業中位數 15% 以上 或 處於強勢區間。
    - `🟡`：處於同業中位數 ±15% 或 處於中性區間。
    - `🔴`：劣於同業中位數 15% 以上 或 處於危險/超賣區間。
- **數據精度**：P/E 保留至小數點第一位；百分比保留至小數點第一位。

## 4. 執行流 (Execution Flow)
1. **數據採集** → `yfinance` → `ta` (量化指標) → `web_search` (最新新聞)。
2. **量化比對** → 計算標的與同業 (Peers) 的偏差百分比。
3. **模版填充** → 將數值注入上述模版 → 根據數值自動選擇燈號。
4. **最終校對** → 確保無 HTML 標籤，僅使用 Markdown 符號。

## 5. 格式禁令
- **禁止 LaTeX**：嚴禁使用 `→`、`...` 等 LaTeX 語法。全部改用 Unicode 符號（→, ⇒, ±, %）。
- **原因**：Telegram 不支援 LaTeX 渲染，LaTeX 會顯示為原始碼或亂碼。
