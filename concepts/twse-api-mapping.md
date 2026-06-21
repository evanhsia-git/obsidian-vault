---
name: twse-api-mapping
description: 台灣證券交易所 (TWSE) OpenAPI 端點映射與抓取規範。
category: data-source
title: Twse-Api-Mapping
summary: Twse-Api-Mapping：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[concepts/stock-database-state|股票資料庫狀態]]
- [[concepts/stock-automation-config|股票自動化配置]]


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