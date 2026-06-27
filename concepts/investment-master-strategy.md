---
title: "投資策略與股市資料比較"
description: "投資大師選股策略 + 股市資料獲取比較 + Manus 案例"
summary: "投資大師策略 + 資料比較表 + Agent 資料分工 + Manus 升級建議"
type: concept
status: active
tags: [finance, stock]
created: 2026-05-31
updated: 2026-06-27
---

# 投資策略與股市資料比較

## 投資大師選股策略速查

| 投資人類型 | 代表 | 核心策略 | 關鍵指標 |
|:---|:---|:---|:---|
| 動能 | 墨菲、米尼維尼、達華斯、李佛摩、德勒克米勒 | 順勢交易、嚴格停損 | 價格動能、成交量、箱型突破 |
| 價值 | 巴菲特、蒙格、卡拉曼、格拉罕、奈夫、坦伯頓 | 安全邊際、低估值 | PE、PB、護城河 |
| 成長 | 林區、費雪、歐尼爾、伍德 | 顛覆式創新、成长股 | GPEG、ROIC、營收成長 |
| 反向 | 科斯托蘭尼、伯里、伊坎、索羅斯 | 逆向投資、反身性 | 市場心理、總經 |
| 量化 | 西蒙斯、葛林布雷、達里歐 | 模型、全天候 | 數學公式、資產配置 |

## 股市資料獲取比較

| 優先 | 來源 | 功能 | 限制 |
|:---|:---|:---|:---|
| 1 | TWSE OpenAPI | 台股日價/財報 | 批次每日1次 |
| 2 | TPEX OpenAPI | 上櫃/興櫃 | 同上 |
| 3 | FinMind | 法人/融資/月營收 | 100~600 req/hr |
| 4 | OpenBB | 統一介面 | 部分付費 |
| 5 | yfinance | 美股/ETF | 中 stability |

### 注意事項
- CSV 編碼 UTF-8-BOM
- 三大法人 16:00-17:00 更新
- 請求間隔 > 100ms

### Agent 資料分工

| 資料 | 來源 |
|:---|:---|
| 台股/OTC 股價 | TWSE/TPEX |
| 月營收/財報/法人 | FinMind |
| 美股 ETF | yfinance |
| 美國財報 | FMP |
| 新聞 | Finnhub |
| 宏觀 | OpenBB |

## Manus 案例對照（3 項行動方案）

1. **SQLite 私有數據層**（最高優先）：`get_stock_price()` / `get_financials()` / `get_pe_pb()`
2. **自動化投資備忘錄**（中優先）：摘要模式 + Infographic + Email
3. **財報異常偵測看門狗**（中優先）：每月 Cron 掃描 ±20% 異常 → Telegram

## Manus vs Hermes

| 面向 | Manus | Hermes |
|:---|:---|:---|
| 技能 | Batch 安裝 | SKILL.md 手動 |
| 部署 | 全端 Web App | Terminal |
| 案例 | 278 社群案例 | ~50 個人技能 |
