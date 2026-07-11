---
title: "每日新聞 RSS 來源清單"
description: "每日新聞 RSS 來源管理清單 — 包含台股、美股、科技與一般新聞的 RSS URL 列表"
summary: "每日新聞 RSS 來源清單"
type: resource
status: active
tags: [rss, source, technology, market, daily-news]
created: 2026-07-09
updated: 2026-07-09
---

# 每日新聞 RSS 來源清單

## 🇹🇼 台股新聞來源
| 來源 | RSS URL |
|------|---------|
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

## 🇺🇸 美股與宏觀
| 來源 | RSS URL | 狀態 |
|------|--------|------|
| Bloomberg Markets | `https://feeds.bloomberg.com/markets/news.rss` | ✅ |
| Bloomberg Business | `https://feeds.bloomberg.com/business/news.rss` | ✅ |
| Bloomberg Technology | `https://feeds.bloomberg.com/technology/news.rss` | ✅ |
| Bloomberg Bview | `https://feeds.bloomberg.com/bview/news.rss` | ✅ |
| Yahoo 美股 | `https://tw.stock.yahoo.com/rss?category=us-market` | ✅ |
| 鉅亨網 | `https://news.cnyes.com/rss/v1/news/category/wd_stock` | ✅ |

## 🤖 科技新聞
### 國際
- TechCrunch (`https://techcrunch.com/feed/`)
- The Verge (`https://www.theverge.com/rss/index.xml`)
- Ars Technica (`https://www.arstechnica.com/rss/`)
- Wired (`https://www.wired.com/feed/rss`)
- MIT Technology Review (`https://www.technologyreview.com/feed/`)

### 台灣
- TechNews 科技新報 (`https://technews.tw/feed/`)
- INSIDE (`https://www.inside.com.tw/feed/rss`)
- iThome (`https://www.ithome.com.tw/rss`)
- 科技報橘 TechOrange (`https://techorange.com/feed/`)
- DIGITIMES (`https://www.digitimes.com.tw/tech/rss/xml/xmlrss_30_25.xml`)
- 中央社-科技 (`https://feeds.feedburner.com/rsscna/technology`)
- 電腦王阿達 (`https://www.koc.com.tw/feed`)
- 36氪 (`https://36kr.com/feed`)
- IT之家 (`https://www.ithome.com/rss/`)
- 少數派 (`https://sspai.com/feed`)
- 愛范兒 (`https://www.ifanr.com/feed`)
- 虎嗅網 (`https://rss.huxiu.com/`)
- V2EX (`https://v2ex.com/index.xml`)
- 量子位 (`https://www.qbitai.com/feed`)
- CoolShell 酷殼 (`http://coolshell.cn/feed`)
- 雷鋒網 (`https://www.leiphone.com/feed`)
- 美團技術團隊 (`https://tech.meituan.com/feed`)
- 經部產業技術司 (`https://www.moea.gov.tw/MNS/doit/news/NewsRSSdetail.aspx?sno=31&Kind=1`)

### 已排除/已停用
> **來源**: **理由**
> - MarketWatch | 擋爬蟲 |
> - Thomson Reuters | 只有 IR 公告 |
> - CNN Top Stories | 2023 舊新聞 |
> - 鉅亨網 m.cnyes | XML 解析錯誤 |
> - iThome RSS | 回傳 2022 舊資料 |

### 備註與維護指引

- **格式**: 所有 URL 均保持原始鏈接；繁體中文標題 + 純中文內容。  
- **驗證**: 每個 RSS 來源均已在 2026-07-09 通過 `curl` 測試（顯示 `✅` 狀態）。  
- **更新**: 如需新增/移除 RSS 來源，請同時更新：
  1. 本清單（此檔案）  
  2. `daily-news-tech.py` 的 `TECH_SRC` 陣列（發佈每日科技新聞）  
  3. 任何使用此 RSS 來源的其他腳本或技能。  

---

### 相關連結

- [[news-and-market-examples|每日台股新聞輸出範例]] → 用於輸出格式 / 資料結構
- [[daily-news-technology|每日科技新聞推送技能]] → 發佈每日科技新聞
- [[daily-news-usstock-fix|每日美股新聞推送技能]] → 發佈每日美股新聞
- 每日系統維護 (Lint+備份+Quartz) → RSS 來源的系統維護記錄

---

### 版本紀錄

| 版本 | 日期 | 修改者 | 備註 |
|------|------|-------|------|
| v1.0 | 2026‑07‑09 | Claude | 初版，建立 RSS 來源統一清單 |
