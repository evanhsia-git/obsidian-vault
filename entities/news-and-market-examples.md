---
title: "News and Market Output Examples"
description: "每日新聞與股市指標的輸出格式範本（Agent 開發與維護規範）"
summary: "新聞格式 + 股市指標輸出範例 — 自動推送參考；RSS 來源請見 daily-news-sources"
type: entity
status: active
tags: [news, source, stock, finance]
created: 2026-06-28
updated: 2026-07-09
---

# News and Market Output Examples

自動化新聞與股市指標推送的格式範本。

> 📡 **RSS 來源總表已移至權威頁**：[[notes/daily-news-sources|每日新聞 RSS 來源清單]]（含台股/美股/科技來源與驗證狀態）。本頁僅保留「輸出格式範例」與「已排除來源」。

---

## 每日新聞 RSS 來源 — 已排除清單

| 來源 | 原因 |
|:---|:---|
| MarketWatch | 擋爬蟲 |
| Thomson Reuters | 只有 IR 公告 |
| CNN Top Stories | 2023 舊新聞 |
| 鉅亨網 m.cnyes | XML 解析錯誤 |
| iThome RSS | 回傳 2022 舊資料 |

---

## 每日股市指標輸出範例

### 完整輸出格式（2026-06-15 更新）

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

### 輸出規範

- **亞洲**：`名稱: 數值` `(日:%,月:%)`
- **美國**：SPY/QQQ `$ 數值 美元` / VIX + `(日:%,月:%)`
- **CNN**：`分數 | [狀態]`
- **宏觀**：匯率 `名稱: 數值 (日:%)` / FRED `名稱: 數值`

### 資料來源

| 指標 | 來源 | 代碼 |
|:---|:---|:---|
| 指數/VIX | Yahoo Finance | ^TWII/^N225/^KS11/^VIX/SPY/QQQ |
| 匯率 | Yahoo Finance | TWD=X |
| CNN | CNN Dataviz | fearandgreed/graphdata |
| EFFR/CPI/GDP | FRED | EFFR/CPIAUCSL/GDP |

### 維護規範

- 新增來源：同時更新 Python 腳本 `RSS_SOURCES` 陣列（詳見 [[notes/daily-news-sources|RSS 來源清單]]）
- RSS 格式檢查：需正確回傳 XML，標題/摘要無過多 HTML
- 100% 繁體中文原則
- 驗證日期：2026-06-16

---

## 相關頁面

- [[notes/daily-news-sources|每日新聞 RSS 來源清單]] — RSS 權威頁
- [[entities/entities-index]]
- [[concepts/external-services-integration]]
- [[finance/finance-index]]
