---
name: sp500-components
description: S&P 500 成分股資料來源（來自 SlickCharts）。
category: data-source
tools:
  - terminal
  - memory
  - execute_code
title: Sp500-Components
summary: "S&P 500 成分股列表與資料取得方式"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[concepts/fred-economic-data|Fred 經濟數據]]


# S&P 500 成分股 (來自 SlickCharts)

## 概述
SlickCharts 提供 S&P 500 指數成分股的最新列表，按市值進行排序，連結至個股詳細資料頁面。

## 資料來源
- **網址**：https://www.slickcharts.com/sp500
- **類型**：實時股票清單頁面
- **更新頻率**：每交易日（依據 S&P Dow Jones Indices 每月調整）

## 核心欄位
| 欄位 | 說明 |
|------|------|
| Ticker | 股票代號 |
| Company | 公司名稱 |
| Price | 目前價格 |
| Change | 漲跌點數 |
| % Change | 漲跌百分比 |
| Market Cap | 總市值 (由 SlickCharts 計算) |
| Weight | 在 S&P 500 指數中的權重（百分比） |
| Sector | 所屬產業分類 |

## 資料使用說明
- **價格與權重監控**：追蹤大型成分股的價格變化與權重變化，了解市場資金流向。
- **成分股篩選**：快速取得潛在分析標的（如高市值、高權重的科技股）。
- **權重變化警示**：當權重超過預期（如 AAPL 權重突破 7%），可作為盤中交易警示。

## 與本系統的關聯
- 屬於 **T2 - Financial Media (可信度: MEDIUM)** 數據源。
- 可作為 `wall-street-portfolio-manager` 中 `MARKET_EVENTS` 或主題分析 (`THEME_EVENTS`) 的數據基礎。

## 使用範例 (擷取標頭 10 行)
| Ticker | Company | Price | Change | % Change | Market Cap | Weight | Sector |
|--------|---------|-------|--------|----------|------------|--------|--------|
| AAPL | Apple Inc | 172.50 | +0.95 | +0.55% | $2.7T | 6.98% | Technology |
| MSFT | Microsoft Corp | 335.10 | +1.20 | +0.36% | $2.5T | 5.89% | Technology |
| GOOGL | Alphabet Inc | 157.25 | -0.45 | -0.29% | $1.8T | 4.32% | Communication Services |
| AMZN | Amazon.com Inc | 132.80 | +2.10 | +1.61% | $1.6T | 3.98% | Consumer Discretionary |
| META | Meta Platforms Inc | 410.50 | -1.30 | -0.32% | $1.3T | 3.45% | Communication Services |
| ... | ... | ... | ... | ... | ... | ... | ... |

## 擷取方式
可手動瀏覽 https://www.slickcharts.com/sp500，或開發爬蟲腳本擷取數據，並匯入至 DuckDB/Obsidian 資料庫。

## 相關參考
- S&P 500 指數介紹：[Wikipedia - S&P 500](https://en.wikipedia.org/wiki/S%26P_500)
- 指數權重規則：[S&P Dow Jones Indices](https://www.spglobal.com/spdji/)