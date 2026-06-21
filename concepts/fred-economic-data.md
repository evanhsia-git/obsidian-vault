---
name: fred-economic-data
description: 美國聯準會總體經濟資料庫 (FRED) 數據源。
category: data-source
tools:
  - terminal
  - memory
  - execute_code
title: Fred-Economic-Data
summary: Fred-Economic-Data：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
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