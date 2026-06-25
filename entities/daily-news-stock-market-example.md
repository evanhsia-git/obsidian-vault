---
title: "每日股市指標輸出範例"
description: "每日股市指標輸出範例 — 實體資料頁面"
summary: "每日股市指標輸出範例"

type: entity
status: active
priority: P2

tags: []
aliases: []

created: 2026-06-16
updated: 2026-06-16
date: 2026-06-16

publish: true
draft: false

related:
source:

due:
review:
---
# 📋 每日股市指標輸出範例
|---
|name: daily-news-stock-market
|title: 每日股市指標輸出範例
|description: 存放每日股市指標報告的輸出範本，用於對齊開發與維護規範。
|updated: 2026-06-15
|type: entity
|tags: [stock, reporting, template]
|---

## 完整輸出範例
(依據 2026-06-15 更新之最新範本)

亞洲核心指數
台灣加權指數(^TWII): 45,396.99
(日: +2.78%,月: +10.26%)

元大台灣50(0050.TW): 105.25
(日: +3.24%,月: +10.32%)

主動統一台股增長(00981A.TW): 31.47
(日: +2.84%,月: +10.73%)

主動統一全球創新(00988A.TW): 22.36
(日: +7.09%,月: +14.55%)

日經225 (Nikkei) (^N225): 69,317.50
(日: +4.99%,月: +12.88%)

韓國 KOSPI (^KS11): 8,545.98
(日: +5.20%,月: +8.95%)

===========

美國核心指標
S&P 500 (SPY): $ 741.75 美元
(日: +0.54%,月: -0.08%)

NASDAQ (QQQ): $ 721.34 美元
(日: +0.59%,月: +0.93%)

VIX 恐慌指數: 16.71
(日: -5.49%,月: -9.33%)

===========

CNN Fear & Greed Index
34 | [恐懼]
(說明：需結合市場情緒變化進行解析)

===========

宏觀與匯率
USD/TWD 匯率: 31.51
(日: -0.20%,月: N/A)

美聯儲有效聯邦基金利率 (EFFR): 3.62
(說明：需結合政策與經濟趨勢進行解析)

美國 CPI: 333.98
(說明：需結合政策與經濟趨勢進行解析)

美國 GDP: 31,819.46
(說明：需結合政策與經濟趨勢進行解析)

===========

股市整體結論
以上為當日最新可得之指標數據，請依亞股漲跌、VIX 水準與匯率走勢綜合研判市場情緒與避險傾向。

(註：報告中 CNN Fear & Greed 數據若顯示失敗，係因 API 讀取限制 [HTTP 418])

---

## 資料來源

| 指標 | 來源 | 代碼 / 端點 |
|------|------|-------------|
| TAIEX / Nikkei / KOSPI / VIX | Yahoo Finance | `^TWII` / `^N225` / `^KS11` / `^VIX` |
| S&P 500 / NASDAQ | Yahoo Finance | `SPY` / `QQQ` |
| USD/TWD | Yahoo Finance | `TWD=X` |
| CNN Fear & Greed | CNN Dataviz | `https://production.dataviz.cnn.io/index/fearandgreed/graphdata` |
| 美聯儲利率 (EFFR) | FRED API | `EFFR` |
| CPI / GDP | FRED API | `CPIAUCSL` / `GDP` |

## 技術細節
- **腳本路徑**: `/root/.hermes/skills/user/daily-news-stock-market/scripts/daily-news-stock-market.py`
- **輸出規範 (2026-06-15 更新)**: 
    - **亞洲指數**：主行不含漲跌幅，統一格式為 `名稱: 數值` \n `(日: 漲跌%,月: 漲跌%)`。
    - **美國指標**：
        - SPY/QQQ：主行格式為 `名稱: $ 數值 美元`。
        - VIX：主行格式為 `名稱: 數值`。
        - 下方皆附帶 `(日: 漲跌%,月: 漲跌%)`。
    - **CNN Fear & Greed**：格式為 `分數 | [繁體狀態]`，並附帶固定說明文字。
    - **宏觀與匯率**：
        - USD/TWD：主行不含漲跌幅，漲跌資訊放入下方括號 `(日: 漲跌%,月: N/A)`。
        - FRED 數據：主行 `名稱: 數值`，下方附帶固定說明文字。
- **超時**: 硬超時 120 秒

---
## 相關節點
- [[daily-news-technology-example]]
- [[daily-news-twstock-example]]