---
name: stock-database-state
description: 股票量化資料庫之現況、結構與覆蓋率紀錄。
category: database
title: Stock-Database-State
summary: Stock-Database-State：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[concepts/twse-api-mapping|TWSE API 映射]]
- [[concepts/stock-automation-config|股票自動化配置]]


# 股票量化資料庫狀態 (2026-05-28)

## 1. tw_stock_all.db
- **主要表格**: `stock_overview` (1611 檔)
- **核心指標覆蓋率**:
    - ROE: 65.9%
    - EPS: 49.6% (daily_prices 表)
- **資料清洗規則**: 只保留上市 (twse) 股票，已刪除上櫃 (tpex) 與權證資料。

## 2. tw_stock_top300.db
- **結構**: 包含 15 個表格。
- **ETF 數據**: `etf_basic_info` (256 筆)，含殖利率及基本資訊。
- **清洗狀態**: 已清除所有 tpex_* 前綴表格。

## 3. value_screening.db
- **樣本數**: 3152 筆。
- **關鍵指標覆蓋率**:
    - PE: 56%
    - ROE: 34.1%
    - 淨利率: 33.7%
    - 負債比: 34.3%

## 4. 資料更新紀錄
- **FinMind 限制**: Free tier 覆蓋率接近 0%，無法大量補齊 ROE。
- **TWSE OpenAPI 突破**: 發現 `t187ap06_L_ci` 等端點可一次抓取全部上市股票，目前為最佳補齊路徑。