---
title: "每日股市指標輸出範例"
description: "每日股市指標報告輸出範本（Agent 開發與維護規範）"
summary: "股市指標輸出格式：亞洲指數 + 美國指標 + CNN + 宏觀匯率"
type: entity
status: active
tags: [stock, finance]
created: 2026-06-16
updated: 2026-06-27
---

# 每日股市指標輸出範例

## 完整輸出範例（2026-06-15 更新）

**亞洲核心指數**
台灣加權(^TWII): 45,396.99 (日:+2.78%,月:+10.26%)
日經225(^N225): 69,317.50 (日:+4.99%,月:+12.88%)
韓國KOSPI(^KS11): 8,545.98 (日:+5.20%,月:+8.95%)

**美國核心指標**
S&P 500(SPY): $741.75 (日:+0.54%,月:-0.08%)
NASDAQ(QQQ): $721.34 (日:+0.59%,月:+0.93%)
VIX: 16.71 (日:-5.49%,月:-9.33%)

**CNN Fear & Greed**: 34 | [恐懼]

**宏觀與匯率**
USD/TWD: 31.51 (日:-0.20%)
EFFR: 3.62 | CPI: 333.98 | GDP: 31,819.46

## 輸出規範

- **亞洲**：`名稱: 數值` `(日:%,月:%)`
- **美國**：SPY/QQQ `$ 數值 美元` / VIX `數值` + `(日:%,月:%)`
- **CNN**：`分數 | [狀態]`
- **宏觀**：匯率`名稱: 數值` + `(日:%)` / FRED `名稱: 數值`

## 資料來源

| 指標 | 來源 | 代碼 |
|:---|:---|:---|
| 指數/VIX | Yahoo Finance | ^TWII/^N225/^KS11/^VIX/SPY/QQQ |
| 匯率 | Yahoo Finance | TWD=X |
| CNN | CNN Dataviz | fearandgreed/graphdata |
| EFFR/CPI/GDP | FRED | EFFR/CPIAUCSL/GDP |

- 腳本：`skills/daily-news-stock-market/scripts/daily-news-stock-market.py`
- 超時：120 秒
