---
title: "每日新聞來源管理清單 (RSS)"
description: "自動化新聞蒐集腳本的 RSS 來源總表"
summary: "RSS 來源：台股/美股/科技/一般新聞 + 驗證狀態 + 維護指南"
type: entity
status: active
tags: [news, source]
created: 2026-06-15
updated: 2026-06-27
---

# 每日新聞來源管理清單 (RSS)

## 🇹🇼 台股新聞來源

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

## 🇺🇸 美股與宏觀財經

### Bloomberg（✅ 驗證可用，30 則即時）

| 來源 | RSS URL | 狀態 |
|:---|:---|:---|
| Markets | `https://feeds.bloomberg.com/markets/news.rss` | ✅ |
| Business | `https://feeds.bloomberg.com/business/news.rss` | ✅ |
| Technology | `https://feeds.bloomberg.com/technology/news.rss` | ✅ |
| Bview | `https://feeds.bloomberg.com/bview/news.rss` | ✅ |
| Wealth | `https://feeds.bloomberg.com/wealth/news.rss` | ❌ 0則 |
| Gadfly | `https://feeds.bloomberg.com/gadfly/news.rss` | ❌ 0則 |

### 中文/備用
- Yahoo 美股：`https://tw.stock.yahoo.com/rss?category=us-market`
- 鉅亨網：`https://news.cnyes.com/rss/v1/news/category/wd_stock`
- 英文備用：CNBC, WSJ, FT, Yahoo Finance EN, Investing.com, Seeking Alpha

## 🤖 科技新聞來源

### 國際
- Bloomberg Technology/Markets/Business/Bview（同上方）
- TechCrunch, The Verge, Ars Technica, Wired, MIT Tech Review

### 台灣
- 科技新報：`https://technews.tw/feed/`
- INSIDE：`https://www.inside.com.tw/feed/rss`
- iThome：`https://www.ithome.com.tw/rss`
- 科技報橘：`https://techorange.com/feed/`

## 📰 一般新聞來源（中央社 + 國際中文）

- 中央社 9 個分類：politics/intworld/mainland/lifehealth/social/local/culture/sport/stars
- 自由時報國際：`https://news.ltn.com.tw/rss/world.xml`
- 公視：`https://news.pts.org.tw/xml/newsfeed.xml`
- 國際中文：NYTimes/BBC/RFI/DW/Sputnik/Chosun/共同網/日經 (feedx.net)

## ❌ 已排除來源

| 來源 | 原因 |
|:---|:---|
| MarketWatch | 擋爬蟲，0 則 |
| Thomson Reuters | 只有 IR 公告 |
| CNN Top Stories | 2023 舊新聞 |
| 鉅亨網 m.cnyes.com | XML 解析錯誤 |
| iThome RSS | 回傳 2022 舊資料 |

## 📚 維護指南

1. **新增來源**：同時更新 Python 腳本 `RSS_SOURCES` 陣列
2. **格式檢查**：RSS 需正確回傳 XML，標題/摘要無過多 HTML
3. **禁止**：未經驗證的英文來源（100% 繁體中文原則）
4. **驗證日期**：2026-06-16
