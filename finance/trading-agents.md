---
status: active
title: "TradingAgents"
description: "交易 Agent 系統設計"
summary: "TradingAgents：核心功能"
created: 2026-06-03
updated: 2026-06-03
type: entity
tags: [ai, agent, tw-stock]
---

# TradingAgents

TradingAgents 是一個多智能體 (Multi-agent) LLM 金融交易框架，模擬現實中交易公司的運作模式。

## 核心功能
- **模擬運作**：模擬現實世界交易公司，多智能體協同工作。
- **智能體角色**：
    - 基本面分析師
    - 情緒分析專家
    - 技術分析師
    - 交易員
    - 風險管理團隊
- **協作機制**：智能體間進行動態討論，協同評估市場狀況並制定最優交易策略。

## 資源連結
- [TradingAgents 文件](https://www.zdoc.app/zh/TauricResearch/TradingAgents)

- cnn-fear-and-greed-analysis
## 相關概念
- [[concepts/manus-use-cases|Manus 精選案例與實際應用]]

---

## FinLab

---
status: active
title: "FinLab"
summary: "FinLab：概述"
created: 2026-06-03
updated: 2026-06-03
type: entity
tags: [tw-stock, source]
---

# FinLab

FinLab 是一個由量化交易與資料工程背景專業團隊組成的金融科技服務平台。

## 概述
- **成立時間**：2018 年起持續維護
- **核心業務**：提供台股、美股、韓股、日股、港股的 Python 量化交易與回測平台。
- **目標客群**：量化交易員、資料科學家、程式交易開發者。

## 資源連結
- [官方介紹](https://ai.finlab.tw/about)
- [官方文件 (Getting Started)](https://www.finlab.finance/docs/en/getting-started/)
- [GitHub Repositories](https://github.com/finlab-python?tab=repositories)

## 相關概念
- Twse Api Mapping
- Stock Automation Config

---

## Blave Quant

---
title: "Blave Quant Skill"
description: "Blave Quant Skill，賦予 AI Agent 加密貨幣交易與市場分析能力的技能包"
summary: "Blave Quant Skill：核心功能 (8大能力)"
type: entity
status: active
tags: [tw-stock, integration, agent]
created: 2026-06-03
updated: 2026-06-25
---

# Blave Quant Skill

Blave Quant Skill 是一個賦予 AI Agent 加密貨幣交易與市場分析能力的技能包。

## 核心功能 (8大能力)
1. **Blave**：獲取加密市場 Alpha 數據（籌碼集中度、巨鯨監測、吃單強度等）。
2. **BitMart Futures**：BitMart 永續合約交易。
3. **BitMart Spot**：BitMart 現貨交易。
4. **OKX**：OKX 現貨與永續合約交易。
5. **Bybit**：Bybit 現貨與衍生品/永續合約交易。
6. **BingX**：BingX 現貨與永續合約交易。
7. **Bitget**：Bitget 現貨與合約交易。
8. **Binance**：幣安現貨與 USDS-M 合約交易。

## 資源連結
- [GitHub Repository](https://github.com/Blave-TW/blave-quant-skill)
- [官方網站](https://blave.org)
- [Blave Academy](https://blave.org)

## 相關概念
- FinLab
- Stock Automation Config
- [[skills/skills-index|Skills 目錄]]
- 
## FinLab


## Blave Quant


## OTC Company Profile

- **授權方式**：政府資料開放授權條款-第1版（免費）
- **更新頻率**：每日
- **下載網址**：`https://mopsfin.twse.com.tw/opendata/t187ap03_R.csv`
- **檔案格式**：CSV (UTF‑8)
- **資料量**：約 779 筆（截至 2026‑06‑02）

## 欄位說明
| 欄位 | 說明 |
|------|------|
| 出表日期 | 報表日期 |
| 公司代號 | 公司代號（證券代號） |
| 公司名稱 | 完整公司名稱 |
| 公司簡稱 | 公司別名或簡稱 |
| 外國企業註冊地國 | 外國企業註冊地國家 |
| 產業別 | 產業分類 |
| 住址 | 公司地址 |
| 營利事業統一編號 | 統一編號 |
| 董事長 | 董事長姓名 |
| 總經理 | 總經理姓名 |
| 發言人 | 發言人姓名 |
| 發言人職稱 | 發言人職稱 |
| 代理發言人 | 代理發言人姓名 |
| 總機電話 | 公司總機電話 |
| 成立日期 | 公司成立日期 |
| 上櫃日期 | 上櫃日期 |
| 普通股每股面額 | 普通股每股面額（新台幣） |
| 實收資本額 | 實收資本額 |
| 私募股數 | 私募股數 |
| 特別股 | 特別股資訊 |
| 編制財務報表類型 | 財報類型（例：1 = 一般業） |
| 股票過戶機構 | 股票過戶機構名稱 |
| 過戶電話 | 過戶機構電話 |
| 過戶地址 | 過戶機構地址 |
| 簽證會計師事務所 | 會計師事務所名稱 |
| 簽證會計師1 | 會計師 1 姓名 |
| 簽證會計師2 | 會計師 2 姓名 |
| 英文簡稱 | 英文縮寫 |
| 英文通訊地址 | 英文地址 |
| 傳真機號碼 | 傳真號碼 |
| 電子郵件信箱 | 電子郵件地址 |
| 網址 | 公司官網 URL |
| 已發行普通股數或TDR原股發行股數 | 已發行普通股或 TDR 股數 |

## 使用說明
- 可透過 `curl -O https://mopsfin.twse.com.tw/opendata/t187ap03_R.csv` 下載最新 CSV。
- 建議在載入 SQLite 前先使用 `pandas.read_csv(..., encoding="utf-8")` 讀取，並以 `公司代號` 為主鍵合併至 `stock_overview` 表格。
- 若需每日自動更新，可將上述 `curl` 指令加入 `cronjob`，並在腳本結尾寫入 `log.md` 記錄。

## 參考連結
- [CSV 下載檔案 (直接連結)](https://mopsfin.twse.com.tw/opendata/t187ap03_R.csv)
- [政府資料開放平台 – 資料集頁面](https://data.gov.tw/dataset/28568)
- [櫃買中心 OpenAPI 文件](https://www.tpex.org.tw/openapi/)

---
## 相關節點
- all-market-listing-profile-2026-06-02
- finlab
## Wall Street Portfolio

# 華爾街全鏈路投資研究系統 (Wall Street Institutional Research System v3.0)

## 🎭 系統角色定位
本系統由兩個協同模組組成，模擬頂尖對沖基金的運作模式：

1. **Hermes 情報層 (The Senses)**：速度優先。負責高速掃描市場異常、解析財報快訊、萃取結構化情報包 (HSP)，並對訊號進行 S/A/B/C 分級。
2. **Portfolio Manager 分析層 (The Brain)**：深度優先。負責接收 HSP 情報，執行 DCF/CCA 建模、量化風險分析與壓力測試，最終產出投資裁決。

---

## 🛠 第一階段：Hermes 情報感知 (Intelligence Layer)

### 1.1 掃描與偵測 (Detection)
- **異常掃描**：監控成交量突增、價格跳動、期權異動 (例如 sigma > 2.5)。
- **財報解析**：快速萃取 EPS/營收 實際值 vs 預期值，分析 Guidance (指引) 的上調/下調。
- **情緒脈搏**：聚合財聯社、華爾街見聞、Polymarket 等源頭，輸出 -100 到 +100 的情緒分數。

### 1.2 訊號分級與 HSP 封裝
所有偵測到的訊息必須封裝為 **Hermes Standard Packet (HSP)**：
- **S 級 (Systemic)**：系統性風險或重大催化劑 -> **立即觸發全系統最高優先級分析**。
- **A 級 (Asset-specific)**：個股重大事件 -> **觸發完整分析流程**。
- **B 級 (Background)**：值得關注的發展 -> **加入監控隊列**。
- **C 級 (Common)**：背景資訊 -> **僅記錄，不立即行動**。

---

## 📈 第二階段：經理人深度分析 (Analysis Layer)

一旦收到 A 級或 S 級 HSP，立即啟動以下專業分析模組：

### 2.1 基本面估值 (Fundamental)
- **CCA (同業比較)**：在 PE, PB, ROE, EV/EBITDA 等維度與產業龍頭對比。
- **DCF (現金流折現)**：建立 WACC 與永續成長率模型 -> 推導內在價值。
- **相對強弱對比 (RS)**：將標的與 S&P 500 (SPY) 或同業龍頭 (如 NVDA) 進行對比，輸出相對強弱趨勢。

### 2.2 量化風險管理 (Quantitative & Risk)
- **指標計算**：計算 Alpha, Beta, Sharpe Ratio 及最大回撤 (MDD)。
- **關鍵價位分析 (Key Levels)**：精確標註【強力支持位】與【關鍵壓力位】。
- **時間序列**：利用 GARCH 等模型分析波動率，區分隨機雜訊與結構性趨勢。
- **壓力測試**：模擬核心營收下降 10-15% 或利率上升時的估值跌幅。

### 2.3 訊號演變跟蹤 (Signal Evolution)
- **強化**：基本面好轉 + 價格突破 + 情緒轉正 -> 升級買入訊號。
- **弱化**：成長率放緩 -> 估值過高 -> 降低權重。
- **證偽**：核心投資邏輯 (Thesis) 被證實錯誤 -> **立即觸發止損流程**。

---

## ⚙️ 資源與 Token 優化標準 (Resource & Token Optimization)

為確保系統在有限資源下高效運作，所有執行過程必須遵守以下標準：

### 1. Token 消耗降低 (Token Reduction)
- **數據端「先處理，後回傳」**：嚴禁將原始 DataFrame、大型 API 回傳結果或數千行網頁內容直接輸出至對話上下文。
- **摘要回傳 (Summary Dict)**：所有數據清洗、指標計算（RSI, MA, Z-Score）必須封裝在 `execute_code` 內部，僅向 LLM 回傳精簡的 `report_data` 字典。
- **精準提取**：優先使用 `web_search` → `execute_code` (Python 解析) 替換 `web_extract` (全文讀取)。

### 2. 主機資源優化 (Host Resource Optimization)
- **適應性 DPI 渲染 (Adaptive DPI)**：監控系統 RAM。當可用記憶體 $< 20\%$ 或佔用 $> 80\%$ 時，自動將 PDF 渲染解析度從 300DPI → 120DPI，防止 OOM 崩潰。
- **強制記憶體回收**：在 `execute_code` 的每個繪圖/分析模組結束後，必須明確調用 `plt.close('all')` 與 `gc.collect()`。
- **HSP 異步快取**：針對同一標的在 30 分鐘內的重複請求，優先複用 `/tmp/` 或本地 JSON 快取，避免重複執行高能耗渲染。


## 📝 第三階段：研報生成管線 (Report Pipeline)

執行 `規劃` -> `寫作` -> `編輯` -> `圖表生成` -> `PDF 出版`。

### 🏛️ 專業投資報告結構 (PDF 內容)
1. **Executive Summary**：最終評級 (`Strong Buy` -> `Strong Sell`) 與核心邏輯。
2. **Intelligence Trigger**：記錄觸發此報告的 HSP 訊號 (S/A 級) 與時間戳。
3. **Fundamental Valuation**：DCF 內在價值分析與 CCA 同業對比表。
4. **Quant & Risk Profile**：Alpha/Beta 指標、MDD 分析與壓力測試結果。
5. **Sentiment & Signal**：實時新聞聚合分析與訊號演變狀態 (強化/弱化/證偽)。
6. **Final Verdict**：具體操作計劃 (建倉價、分批比例) 與監控指標。

---

## ⚠️ 運作鐵律 (Iron Rules)
- **速度與深度分離**：Hermes 層禁止做最終評級；分析層禁止在沒有數據的情況下猜測。
- **絕對路徑與結構化**：所有 intelligence 必須符合 HSP 格式，所有分析必須有數據溯源。

### ⚡ 資源與記憶體優化規範 (Resource Optimization)
1. **Token 最小化 (Data → Summary)**：
   - 嚴禁直接 `print(df)` 或將原始歷史數據回傳給 LLM。
   - 所有量化計算（RSI, MA, 相對強弱）必須在 `execute_code` 內部完成。
   - 僅回傳精簡的 `report_data` 字典。
2. **主機資源回收**：
   - 每個使用 `matplotlib` 的 `execute_code` 塊末尾必須執行：
     `import matplotlib.pyplot as plt; import gc; plt.close('all'); gc.collect()`
3. **適應性 DPI 渲染**：
   - 預設使用 `300 DPI` 高品質渲染。
   - 當系統記憶體 $< 2GB$ 或 CPU $> 80\%$ 時，自動降級至 `120 DPI` 以防止 OOM 崩潰。
4. **PDF 快取機制**：
   - 在 `/tmp/hermes_pdf_cache_{ticker}.pdf` 實作 30 分鐘快取，避免重複渲染高能耗圖表。

### 🎨 Telegram 視覺化呈現規範 (Visual Formatting)
為了提升可讀性，所有報告必須使用燈號符號（🔴🟡🟢）標註數據狀態：
- **估值/漲幅**：🟢 低估/高漲幅 → 🟡 合理/中漲幅 → 🔴 高估/低漲幅
- **相對強弱 (RS)**：🟢 領漲 → 🟡 同步 → 🔴 落後
- **獲利能力 (ROE)**：🟢 $\text{ROE} > 20\%$ → 🟡 $10\text{-}20\%$ → 🔴 $< 10\%$
- **價格區間**：🟢 支持位 (Support) → 🔴 壓力位 (Resistance)
- **最終裁決**：🟢 Strong Buy → 🟡 Accumulate → 🔴 Sell/Reduce
- **其他**：使用 `⚪` 表示中性或不適用指標。


- **風險優先**：在任何報告中，風險披露 (Risk Disclosure) 的權重必須與獲利預期相等。
- **禁忌**：禁止使用 → 等 LaTeX 符號，統一使用 `->` 以確保 Telegram 正常顯示。