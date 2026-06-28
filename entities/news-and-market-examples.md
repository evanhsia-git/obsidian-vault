---
title: "News and Market Output Examples"
description: "每日新聞與股市指標的輸出格式範本（Agent 開發與維護規範）"
summary: "RSS 來源管理 + 新聞格式 + 股市指標輸出範例 — 自動推送參考"
type: entity
status: active
tags: [news, source, stock, finance]
created: 2026-06-28
updated: 2026-06-28
---

# News and Market Output Examples

自動化新聞與股市指標推送的格式範本與資料來源管理。

---

## 每日新聞 RSS 來源總表

### 🇹🇼 台股新聞來源

| 來源 | RSS URL |
|:---|:---|
| 臺灣證券交易所 | `https://www.twse.com.tw/rwd/zh/news/feed?type=rss` |
| 奇摩-最新新聞 | `https://tw.stock.yahoo.com/rss?category=news` |
| 奇摩-台股動態 | `https://tw.stock.yahoo.com/rss?category=tw-market` |
| 奇摩-國際財經 | `https://tw.stock.yahoo.com/rss?category=intl-markets` |
| 奇摩-基金動態 | `https://tw.stock.yahoo.com/rss?category=funds-news` |
| 奇摩-研究報導 | `https://tw.stock.yahoo.com/rss?category=research` |
| 自由時報-財經 | `https://news.ltn.com.tw/rss/business.xml` |
| 中央社-產經 | `https://feeds.feedburner.com/rsscna/finance` |
| 商周 | `https://www.businessweekly.com.tw/Event/feedsec.aspx?feedid=12&channelid=10` |
| 投資臺灣 | `https://investtaiwan.nat.gov.tw/showRSS?lang=cht` |

### 🇺🇸 美股與宏觀

| 來源 | RSS URL | 狀態 |
|:---|:---|:---|
| Bloomberg Markets | `https://feeds.bloomberg.com/markets/news.rss` | ✅ |
| Bloomberg Business | `https://feeds.bloomberg.com/business/news.rss` | ✅ |
| Bloomberg Technology | `https://feeds.bloomberg.com/technology/news.rss` | ✅ |
| Bloomberg Bview | `https://feeds.bloomberg.com/bview/news.rss` | ✅ |
| Bloomberg Wealth/Gadfly | — | ❌ 0則 |
| Yahoo 美股 | `https://tw.stock.yahoo.com/rss?category=us-market` | ✅ |
| 鉅亨網 | `https://news.cnyes.com/rss/v1/news/category/wd_stock` | ✅ |

### 🤖 科技新聞

**國際**：TechCrunch, The Verge, Ars Technica, Wired, MIT Tech Review
**台灣**：科技新報、INSIDE、iThome、科技報橘

### 📰 一般新聞（中央社 + 國際中文）

- 中央社 9 分類：politics/intworld/mainland/lifehealth/social/local/culture/sport/stars
- 自由國際、公視、國際中文（NYTimes/BBC/RFI/DW via feedx.net）

### ❌ 已排除來源

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

- 新增來源：同時更新 Python 腳本 `RSS_SOURCES` 陣列
- RSS 格式檢查：需正確回傳 XML，標題/摘要無過多 HTML
- 100% 繁體中文原則
- 驗證日期：2026-06-16

---

## 相關頁面

- [[entities/entities-index]]
- [[concepts/external-services-integration]]
- [[finance/finance-index]]
