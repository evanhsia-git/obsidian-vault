---
status: active
title: "Nav Toor   Financial Analysis Prompts"
description: "NavToor 金融分析工具"
summary: "Nav Toor   Financial Analysis Prompts：相關頁面"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: [deploy]
---

## 相關頁面
- [[concepts/financial-preferences|金融偏好]]
- [[concepts/hermes-configuration|Hermes 配置]]


# Nav Toor: 投資銀行級財務分析 Prompt 彙整

這套 Prompt 旨在將 AI 轉化為頂尖投行 (如 Goldman Sachs, Morgan Stanley) 級別的分析工具，將簡單的摘要提升至私募股權 (Private Equity) 級別的定量分析。

---

## 1. 運營模式與單位經濟效益 (Operating Model & Unit Economics)

### 💡 分析重點
- **極高標準角色 (Persona)**：設定為 「General Atlantic 的成長股投資人」，驅使 AI 關注「可擴展性 (Scalability)」與「風險控制」。
- **自下而上構建 (Bottom-up)**：要求展示營收構建 (Revenue Build) 過程，而非直接給予結果。
- **核心指標分析**：強制分析 CAC (獲客成本) 與 LTV (生命週期價值) 的關係。
- **動態風險管理**：引入情景規劃 (Upside, Base, Downside) 與燃燒率 (Burn Rate) 計算。
- **嚴格時間維度**：第一年每月預測 → 第二、三年每季預測。

### 📝 Prompt 模板
> 您是 General Atlantic 的成長股投資人。我需要了解 [公司名稱] 的詳細經營模式。
> 
> 請提供：
> - **營收預測**：透過客戶、產品或地區等維度進行自下而上的預測
> - **單位經濟指標**：獲客成本、客戶生命週期價值、回報期、每單位的毛利率
> - **群組分析**：不同年齡層的客戶隨時間的表現如何
> - **主要影響因素**：是什麼導致了收入和成本的變動？
> - **情景規劃**：上漲、持平、下跌三種情況的假設
> - **燃燒率**：每月的現金消耗量及資金維持期計算
> - **盈虧平衡分析**：當公司的現金流為正數時
> - **擴張假設**：隨著業務成長，單位經濟效益如何提升
> 
> 採用運營模式來呈現數據，第一年為每月的預測數據，第二至三年則為每季的預測數據。
> 
> 公司：[說明商業模式、現有指標及成長率]

---

## 2. 核心財務分析 Prompt (四大關鍵任務)

### 2.1 現金流量折現模型 (DCF Model)
- **目的**：估算公司目前價值。
- **角色**：高盛高級分析師 (Senior Analyst)。
- **分析核心**：未來 5 年自由現金流 → WACC 折現 → 終端價值 → 敏感度分析。
- **產出**：牛市 (Bull)、基準 (Base)、熊市 (Bear) 的估值範圍。

### 2.2 三表財務模型 (Three-Statement Model)
- **目的**：分析財務互連性。
- **角色**：摩根士丹利副總裁 (Vice President)。
- **分析核心**：損益表 → 資產負債表 → 現金流量表 的聯動邏輯，包含營運資本與債務還款計畫。
- **產出**：完整 5 年財務預測模型及 Excel 格式邏輯說明。

### 2.3 可比公司分析 (Comparable Company Analysis / Comps)
- **目的**：透過同業定價參照公司價值。
- **角色**：花旗銀行股票研究分析師 (Equity Research Analyst)。
- **分析核心**：篩選 10-15 家對標公司 → 比較 EV/EBITDA、EV/Revenue、P/E 倍數 → 計算溢價/折價原因。
- **產出**：包含分位數 (25th, Median, 75th) 的估值對比表。

### 2.4 IPO 估值與定價分析 (IPO Valuation)
- **目的**：確定上市定價策略。
- **角色**：巴克萊資本市場銀行家 (Capital Markets Banker)。
- **分析核心**：Pre-Money/Post-Money 估值 → 稀釋分析 → 流通盤分析 → 定價區間。
- **產出**：IPO 定價備忘錄 (Pricing Memo) 及建議價格區間。

---

## 🚀 核心邏輯總結
1. **角色設定** → 定義專業深度 (專業角色 = 專業框架)。
2. **推演過程** → 強制自下而上 (Bottom-up) 構建，避免 AI 隨意捏造數字。
3. **動態變數** → 使用情景規劃與壓力測試 (Scenario Planning)。
4. **交付標準** → 模仿頂尖投行 (IB) 的報告格式與時間粒度。

相關頁面：[[environment-keys|Environment Keys]]

相關頁面：[[news-push-log]]


## 相關節點
- [[index]]

---

## Unified Stock Schema

---
status: active
title: "統一股票資料欄位 Schema"
summary: "統一股票資料欄位 Schema：1. 統一 Schema 對照表"
created: 2026-06-04
updated: 2026-06-04
type: concept
tags: [source, tw-stock, deploy]
---

# 統一股票資料欄位 Schema

為確保系統資料庫 (SQLite) 與各資料來源 (TWSE, TPEx, FinMind, yfinance, OpenBB) 運作順暢，所有資料入庫前必須映射為以下統一 Schema。

## 1. 統一 Schema 對照表

| 統一欄位名稱 | 型別 | 描述 | 對應來源建議 |
| :--- | :--- | :--- | :--- |
| `stock_id` | `VARCHAR` | 股票代號 | TWSE/TPEx |
| `date` | `DATE` | 交易日期 (YYYY-MM-DD) | 所有來源 |
| `open` | `FLOAT` | 開盤價 | 所有來源 |
| `high` | `FLOAT` | 最高價 | 所有來源 |
| `low` | `FLOAT` | 最低價 | 所有來源 |
| `close` | `FLOAT` | 收盤價 | 所有來源 |
| `volume` | `BIGINT` | 成交股數 | 所有來源 |
| `amount` | `BIGINT` | 成交金額 | TWSE/TPEx |
| `pe_ratio` | `FLOAT` | 本益比 | TWSE/FinMind |
| `pb_ratio` | `FLOAT` | 股價淨值比 | TWSE/FinMind |
| `dividend_yield` | `FLOAT` | 殖利率 (%) | TWSE/FinMind |
| `eps` | `FLOAT` | 每股盈餘 (元) | FinMind |
| `roe` | `FLOAT` | 股東權益報酬率 (%) | FinMind |

## 2. 轉換規範 (Standardized Logic)
1. **命名規範**：統一使用 `snake_case`，確保與 Pandas/SQLite 欄位索引對齊。
2. **缺值處理 (Handling N/A)**：
   - 數值欄位：無資料填入 `NULL` (SQLite) 或 `None` (Python)。
   - 不建議預設填充 `-1`，以利後續統計聚合 (AVG/SUM) 時避開空值。
3. **日期格式**：所有日期強制轉換為 `YYYY-MM-DD` 字串，並在資料庫內存為 ISO 格式。
4. **編碼對齊**：
   - TWSE/TPEx：優先解析 `UTF-8`；若遇舊 CSV，強制轉換 `utf-8-sig`。
   - 資料處理：所有來源數據進入 SQLite 前需執行 `float()` 型別轉換，並過濾 `-` 或 `空字串`。

## 3. 資料獲取來源優先級 (Source Hierarchy)
- **台股市場**：官方 OpenAPI (TWSE/TPEx) → FinMind (補齊財報/法人指標)。
- **國際/美股/ETF**：OpenBB Platform → yfinance (作為穩定備援)。

## 4. 維護記錄
- 2026-06-04 | 建立統一 Schema 定義檔。

---
## 相關節點
- [[stock-data-sources]]
- [[quant-python-ai-agent]]

## Unified Stock Schema


## Fred Economic Data

status: active
description: "美國聯準會總體經濟資料庫 (FRED) 數據源。"
tags:
  - terminal
  - memory
  - execute_code
title: "Fred-Economic-Data"
summary: "FRED 美國聯準會經濟數據源與操作方法"
created: 2026-05-31
updated: 2026-05-31
type: concept
---

## 相關頁面
- [[concepts/sp500-components|S&P 500 成分]]


# 美國聯準會總體經濟資料庫 (FRED)

## 概述
FRED (Federal Reserve Economic Data) 是由美國聯準會提供的宏觀經濟數據平台，提供超過 900,000 個經濟數據系列，涵蓋:
- 利率（聯邦基金利率、 Policy Rate）
- 通膨指標（消費者物價指數 CPI、核心 CPI）
- 國內生產總值 (GDP) 成長率
- 就業指標（失業率、非農就業人數）
- 貨幣供應量（M2 量寬）
- 匯率、貿易平衡、資本流動等

## 主要數據系列
- `FEDFUNDS`: 監測美國利率變動
- `CPIAUCSL`: 消費者物價指數（CPI）
- `GDP`: 國內生產總值
- `UNEMPLOYEE`: 失業率
- `M2SL`: 貨幣供應 M2
- `EXCHBRATE`: 匯率

## 使用方式
- 透過 Web Interface（https://fred.stlouisfed.org）直接搜尋或下載數據。
- 使用 API（需申請 API Key）進行程式整合：`https://fred.stlouisfed.org/docs/api/fred/`。
- 可透過 Python `fredapi` 套件快速抓取數據。

## 與本系統的關聯
- 屬於 **T1 - Institutional Grade (可信度: HIGH)** 數據源。
- 可用於宏觀事件監控（如 FED_EVENT、RATE_SHOCK）及量化分析。
- 數據質量高，適合作為宏觀事件分析、風險評估與數據可視化的基礎。

## 使用範例
```python
# 示例：抓取近期美國 CPI 數據
import fredapi
fred = fredapi.Fred(api_key='YOUR_API_KEY')
cpi = fred.get_series('CPIAUCSL')
print(cpi.tail())
```