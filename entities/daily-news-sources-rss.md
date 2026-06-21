---
title: 每日新聞來源管理清單 (RSS)
description: 每日新聞來源管理清單 (RSS) — 實體資料頁面
summary: 每日新聞來源管理清單 (RSS)
type: entity
status: published
priority: P2
tags: [data-source, obsidian, maintenance]
aliases: []
created: 2026-06-15
updated: 2026-06-16
date: 2026-06-16
publish: true
draft: false
related:
source:
due:
review:
---

# 📋 每日新聞來源管理清單 (RSS)

本頁面紀錄所有用於自動化新聞蒐集腳本的 RSS Feed 來源。

## 🇹🇼 台股新聞來源

| 來源名稱 | RSS URL |
|---|---|
| 臺灣證券交易所官網 | `https://www.twse.com.tw/rwd/zh/news/feed?type=rss` |
| 奇摩股市-最新新聞 | `https://tw.stock.yahoo.com/rss?category=news` |
| 奇摩股市-台股動態 | `https://tw.stock.yahoo.com/rss?category=tw-market` |
| 奇摩股市-國際財經 | `https://tw.stock.yahoo.com/rss?category=intl-markets` |
| 奇摩股市-小資理財 | `https://tw.stock.yahoo.com/rss?category=personal-finance` |
| 奇摩股市-基金動態 | `https://tw.stock.yahoo.com/rss?category=funds-news` |
| 奇摩股市-專家專欄 | `https://tw.stock.yahoo.com/rss?category=column` |
| 奇摩股市-研究報導 | `https://tw.stock.yahoo.com/rss?category=research` |
| 自由時報-財經 | `https://news.ltn.com.tw/rss/business.xml` |
| 中央通訊社-產經證券 | `https://feeds.feedburner.com/rsscna/finance` |
| 商富財富網最新開放文章 | `https://www.businessweekly.com.tw/Event/feedsec.aspx?feedid=10&channelid=15` |
| 商周最新網站文章 | `https://cmsapi.businessweekly.com.tw/?CategoryId=24612ec9-2ac5-4e1f-ab04-310879f89b33&TemplateId=8E19CF43-50E5-4093-B72D-70A912962D55` |
| 商業週刊最新文章 | `https://www.businessweekly.com.tw/Event/feedsec.aspx?feedid=12&channelid=10` |
| 聚財網最新主題 | `https://feeds.feedburner.com/Wearncom-?format=xml` |
| 聚財網焦點主題 | `https://feeds.feedburner.com/hot_topics_wearncom?format=xml` |
| 商業財富中文網 | `https://plink.anyfeeder.com/fortunechina/shangye` |
| 投資臺灣入口網 | `https://investtaiwan.nat.gov.tw/showRSS?lang=cht` |

## 🇺🇸 美股與宏觀財經

### Bloomberg 國際來源（最高優先，2026-06-16 驗證可用）
| 來源名稱 | RSS URL | 狀態 |
|---|---|---|
| Bloomberg Markets | `https://feeds.bloomberg.com/markets/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Business | `https://feeds.bloomberg.com/business/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Bview | `https://feeds.bloomberg.com/bview/news.rss` | ✅ 28 則，即時更新 |
| Bloomberg Technology | `https://feeds.bloomberg.com/technology/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Politics | `https://feeds.bloomberg.com/politics/news.rss` | ⚠️ 非財經，未使用 |
| Bloomberg Wealth | `https://feeds.bloomberg.com/wealth/news.rss` | ❌ 0 則，已排除 |
| Bloomberg Gadfly | `https://feeds.bloomberg.com/gadfly/news.rss` | ❌ 0 則，已排除 |

### 中文來源
| 來源名稱 | RSS URL |
|---|---|
| Yahoo 美股 | `https://tw.stock.yahoo.com/rss?category=us-market` |
| 鉅亨網全球股市 | `https://news.cnyes.com/rss/v1/news/category/wd_stock` |

### 英文備用來源
- CNBC Top News, Wall Street Journal, Financial Times, Yahoo Finance EN, The Street, Fox Business, Investing.com, Seeking Alpha, Benzinga

### 已排除來源（2026-06-16 測試）
- **MarketWatch**: `https://feeds.content.dowjones.io/public/rss/mw_topstories` — 擋爬蟲，回傳 0 則
- **Thomson Reuters**: `https://ir.thomsonreuters.com/rss/news-releases.xml?items=15` — 只有 IR 公告，非一般新聞
- **CNN Top Stories**: `http://rss.cnn.com/rss/cnn_topstories.rss` — 2023 年舊新聞，非即時

## 🤖 科技新聞來源

### Bloomberg 國際來源（最高優先，2026-06-16 驗證可用）
| 來源名稱 | RSS URL | 狀態 |
|---|---|---|
| Bloomberg Technology | `https://feeds.bloomberg.com/technology/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Markets | `https://feeds.bloomberg.com/markets/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Business | `https://feeds.bloomberg.com/business/news.rss` | ✅ 30 則，即時更新 |
| Bloomberg Bview | `https://feeds.bloomberg.com/bview/news.rss` | ✅ 28 則，即時更新 |

### 台灣中文來源
| 來源名稱 | RSS URL |
|---|---|
| 科技新報 | `https://technews.tw/feed/` |
| INSIDE | `https://www.inside.com.tw/feed/rss` |
| iThome | `https://www.ithome.com.tw/rss` |
| 科技報橘 | `https://techorange.com/feed/` |

### 英文備用來源
TechCrunch, The Verge, Ars Technica, Wired, MIT Tech Review, VentureBeat, ZDNet, CNET

### 已排除來源
- MarketWatch（擋爬蟲）、Thomson Reuters（只有 IR 公告）、CNN（舊新聞）

## 📰 一般新聞來源

| 來源名稱 | RSS URL |
|---|---|
| 中央通訊社-政治 | `https://feeds.feedburner.com/rsscna/politics` |
| 中央通訊社-國際 | `https://feeds.feedburner.com/rsscna/intworld` |
| 中央通訊社-兩岸 | `https://feeds.feedburner.com/rsscna/mainland` |
| 中央通訊社-生活 | `https://feeds.feedburner.com/rsscna/lifehealth` |
| 中央通訊社-社會 | `https://feeds.feedburner.com/rsscna/social` |
| 中央通訊社-地方 | `https://feeds.feedburner.com/rsscna/local` |
| 中央通訊社-文化 | `https://feeds.feedburner.com/rsscna/culture` |
| 中央通訊社-運動 | `https://feeds.feedburner.com/rsscna/sport` |
| 中央通訊社-娛樂 | `https://feeds.feedburner.com/rsscna/stars` |
| 自由時報-國際 | `https://news.ltn.com.tw/rss/world.xml` |
| 公視新聞 | `https://news.pts.org.tw/xml/newsfeed.xml` |
| 紐約時報中文網 | `https://feedx.net/rss/nytimes.xml` |
| BBC News 中文 | `https://feedx.net/rss/bbc.xml` |
| 法國國際廣播電台 | `https://feedx.net/rss/rfi.xml` |
| 德國之聲 | `https://feedx.net/rss/dw.xml` |
| 俄羅斯衛星通訊社 | `https://feedx.net/rss/sputnik.xml` |
| 朝鮮日報網 | `https://feedx.net/rss/chosun.xml` |
| 共同網 | `https://china.kyodonews.net/list/feed/rss4news` |
| 日經中文網 | `https://feedx.net/rss/nikkei.xml` |

---

## 📚 維護指南

1. **新增來源**: 若有新 RSS 來源，需同時更新對應的 Python 腳本（如 `daily-news-twstock.py`）中的 `RSS_SOURCES` 陣列。
2. **格式檢查**: 確保新增的 RSS 能正確回傳 XML 格式，且標題與摘要不含過多 HTML 標籤。
3. **禁止事項**:
    - 禁止在自動化任務中使用未經驗證的英文來源，以確保輸出符合「100% 繁體中文」原則。
    - 避免引入會導致解析錯誤的特殊編碼來源。
4. **已驗證來源（2026-06-16）**：
    - Bloomberg Technology/Markets/Business/Bview：✅ 可用，30 則即時更新
    - 商業週刊：✅ 可用，10 則，深度財經分析（低頻高品質）
    - 鉅亨網 m.cnyes.com（台股/美股）：❌ XML 解析錯誤（bozo=1），已排除
    - MarketWatch / Thomson Reuters / CNN：❌ 已排除（擋爬蟲/IR公告/舊新聞）
5. **相關 Wiki**: [[daily-news-twstock-example|每日台股新聞輸出範例]] · [[daily-news-usstock-example|每日美股新聞輸出範例]]
