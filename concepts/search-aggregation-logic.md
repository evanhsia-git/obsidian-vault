---
status: active
title: "搜尋引擎聚合邏輯說明"
description: "搜尋聚合邏輯實作"
summary: "搜尋引擎聚合邏輯說明：1. 定義"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [auto, source, rag]
---

# 搜尋引擎聚合邏輯 (Search Engine Aggregation Logic)

## 1. 定義
搜尋引擎聚合邏輯是指將多個不同來源（如搜尋引擎、API、資料庫）的查詢結果進行智慧合併、去重、排序與過濾的技術方法。其目標是利用各來源的優勢，彌補單一來源的不足，產出更完整、準確且無偏見的最終結果。

## 2. 金融數據蒐集中的具體應用
在 `ws-data-gatherer` 中，此邏輯主要用於提升數據可靠度：

- **多源股價數據驗證**：同時從 TWSE OpenAPI、Yahoo Finance、FinMind 抓取同一檔股票的收盤價，取中位數或加權平均值減少單點錯誤。
- **新聞內容去重與補充**：從多個財經媒體抓取同一則新聞，自動去重後合併各來源的補充資訊。
- **基本面指標交叉驗證**：比較不同來源提供的 PE/PB/ROE 數值，當偏差超過閾值時觸發複核機制。
- **市場情緒綜合評分**：結合 PTT、新聞標題的情緒分析，產出穩定的綜合情緒指數。

## 3. 主要好處與優勢

| 優勢 | 說明 |
| :--- | :--- |
| **提升準確性** | 透過多源交叉驗證減少單點錯誤，確保數據精確度。 |
| **擴大覆蓋率** | 結合不同來源特性，彌補單一資料來源的缺失。 |
| **時效性互補** | 整合即時新聞價格與深度財報數據，平衡時效與質量。 |
| **抗風險能力** | 當某來源中斷時，系統可自動切換至其他備援來源。 |
| **去除偏見** | 綜合多家報導，平衡單一媒體的立場影響。 |

## 4. 實作建議
在 `ws-data-gatherer` 中建議採取：
1. **並行請求**：同時向多個來源發送請求。
2. **標準化轉換**：將結果轉換為統一的 JSON 格式。
3. **智慧選擇規則**：例如價格取中位數，基本面數據若偏差 < 5% 則取平均值。
4. **可靠度標註**：為每筆聚合數據標記信賴分數。

---
## 相關節點
- stock-data-sources
- unified-stock-schema

## TWSE API Mapping

---
status: active
description: "台灣證券交易所 (TWSE) OpenAPI 端點映射與抓取規範。"
title: "Twse-Api-Mapping"
summary: "Twse-Api-Mapping：相關頁面"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[concepts/stock-database-state|股票資料庫狀態]]
- 股票自動化配置


# TWSE API 映射表 (2026-05-28)

## 1. 個股估值與基本面
| 端點名稱 | URL / 請求路徑 | 內容 | 編碼 | 頻率 | 來源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BWIBBU_ALL** | `exchangeReport/BWIBBU_ALL?response=open_data` | PE + 殖利率 + PB (1077檔上市) | UTF-8 | 每日 | data.gov.tw 11547 |
| **BWIBBU_d** | `exchangeReport/BWIBBU_d?response=csv&date=YYYYMMDD` | 收盤價 + PE + PB + 殖利率 | Big5 | 每日 | TWSE |
| **STOCK_DAY_AVG_ALL** | `exchangeReport/STOCK_DAY_AVG_ALL?response=open_data` | 收盤價 + 月平均價 (26846檔) | UTF-8 | 最新日 | data.gov.tw 11548 |

## 2. 財務報表 (ROE/EPS 核心)
- **損益表 (Income Statement)**: `t187ap06_L_ci`
- **資產負債表 (Balance Sheet)**: `t187ap07_L_ci`
- **注意**：不同產業需使用不同端點（如 _basi, _fh, _ins, _bd）。
- **核心欄位**：`基本每股盈餘（元）`。
- **計算邏輯**：ROE = 稅後淨利 / 股東權益 (需年化 4 季)。

## 3. ETF 數據
- **端點**: `https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv`
- **內容**: 256 檔 ETF, 29 欄位。
- **編碼**: UTF-8 BOM (utf-8-sig)。
- **更新頻率**: 每月。

## 4. 優先級定義
當多個來源提供相同數據時，優先級如下：
**TWSE OpenAPI → TPEX → FinMind → OpenBB**

## TWSE API Mapping


# TWSE API 映射表 (2026-05-28)

## 1. 個股估值與基本面
| 端點名稱 | URL / 請求路徑 | 內容 | 編碼 | 頻率 | 來源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BWIBBU_ALL** | `exchangeReport/BWIBBU_ALL?response=open_data` | PE + 殖利率 + PB (1077檔上市) | UTF-8 | 每日 | data.gov.tw 11547 |
| **BWIBBU_d** | `exchangeReport/BWIBBU_d?response=csv&date=YYYYMMDD` | 收盤價 + PE + PB + 殖利率 | Big5 | 每日 | TWSE |
| **STOCK_DAY_AVG_ALL** | `exchangeReport/STOCK_DAY_AVG_ALL?response=open_data` | 收盤價 + 月平均價 (26846檔) | UTF-8 | 最新日 | data.gov.tw 11548 |

## 2. 財務報表 (ROE/EPS 核心)
- **損益表 (Income Statement)**: `t187ap06_L_ci`
- **資產負債表 (Balance Sheet)**: `t187ap07_L_ci`
- **注意**：不同產業需使用不同端點（如 _basi, _fh, _ins, _bd）。
- **核心欄位**：`基本每股盈餘（元）`。
- **計算邏輯**：ROE = 稅後淨利 / 股東權益 (需年化 4 季)。

## 3. ETF 數據
- **端點**: `https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv`
- **內容**: 256 檔 ETF, 29 欄位。
- **編碼**: UTF-8 BOM (utf-8-sig)。
- **更新頻率**: 每月。

## 4. 優先級定義
當多個來源提供相同數據時，優先級如下：
**TWSE OpenAPI → TPEX → FinMind → OpenBB**