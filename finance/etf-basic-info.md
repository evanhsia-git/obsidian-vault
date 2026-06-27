---
title: "ETF 基本資料與 API 定價"
description: "ETF 基本資料 + Gemini API 定價 + S&P 500 成分"
summary: "ETF 256檔類型統計 + Gemini 模型定價表 + S&P500 來源"
type: resource
status: active
tags: [finance, stock, ai]
created: 2026-05-28
updated: 2026-06-27
---

# ETF 基本資料與 API 定價

## ETF 基本資料（TWSE，2026-05-28）

- 來源：[data.gov.tw/dataset/157399](https://data.gov.tw/dataset/157399)
- API：`GET https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv`
- 256 檔 / 29 欄位 / 每月更新 / 免費

### 類型統計

| 類型 | 數量 |
|:---|:---|
| 國外成分證券指數股票型基金 | 92 |
| 國內成分證券指數股票型基金 | 66 |
| 槓桿/反向 | 51 |
| 國內主動式 ETF | 16 |

### 重要代號

0050(台灣50) / 0051(大中型100) / 0056(高股息) / 0061(寶滬深) / 00631L(正2) / 00632R(反1)

### 注意事項
- 編碼 UTF-8 with BOM
- 日期格式：民國年 MMDD → 西元年 = 民國年 + 1911
- 主動式 ETF 無追蹤指數

---

## Gemini API 定價摘要

### 主要模型（標準輸入，百萬詞元）

| 模型 | 輸入 | 輸出 | 定位 |
|:---|:---|:---|:---|
| gemini-2.5-flash-lite | $0.10 | $0.40 | 最便宜 |
| gemini-3.1-flash-lite | $0.25 | $1.50 | 翻譯/處理 |
| gemini-2.5-flash | $0.30 | $2.50 | 混合推理 |
| gemini-3.1-pro | $2.00 | $12.00 | 最先進 |
| gemini-2.5-pro | $1.25 | $10.00 | 編程/複雜推理 |

### 生成模型

- 最便宜圖片：**Imagen 4 Fast $0.02/張**
- 最便宜影片：**Veo 3.1 Lite $0.05/秒**
- 最便宜的 Embedding：**gemini-embedding-001 $0.15/百萬詞元**

### 降本技巧
- 批次 API 降 50%
- 脈絡快省輸入費
- Flash-Lite 最便宜文字模型

### 本系統使用
- 主模型：`gemma-4-31b-it`（免費 via OpenRouter）
- Vision：`google/gemma-4-31b-it:free`

---

## S&P 500 成分股來源

- 來源：[SlickCharts](https://www.slickcharts.com/sp500)
- 欄位：Ticker / Company / Price / % Change / Market Cap / Weight / Sector
- 用途：價格監控、權重變化警示、成分股篩選
