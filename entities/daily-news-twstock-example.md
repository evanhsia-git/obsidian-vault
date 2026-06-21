---
title: 每日台股新聞輸出範例
description: 每日台股新聞輸出範例 — 實體資料頁面
summary: 每日台股新聞輸出範例
type: entity
status: published
priority: P2
tags: ["taiwan-stock"]
aliases: []
created: 2026-06-11
updated: 2026-06-11
date: 2026-06-11
publish: true
draft: false
related:
source:
due:
review:
---

# 📋 每日台股新聞輸出範例

## Header 格式
```
Cronjob : 每日台灣股市新聞
(job_id: 3b19d0669d5a)
skills: daily-news-twstock
```

## 單筆新聞格式（標準範本）
```
**標題（粗體）**
摘要內容（一句核心描述）
[連結](完整網址)
```
- 第一行：標題，使用 `**粗體**`
- 第二行：摘要，一句核心描述新聞重點
- 第三行：連結，Markdown 格式 `[連結](網址)`，隱藏網址
- 每筆之間以空行分隔

## 完整輸出範例

Cronjob : 每日台灣股市新聞
(job_id: 3b19d0669d5a)
skills: daily-news-twstock

**台積電涉專利侵權！多位共和黨議員要求嚴格執法：不應有特殊待遇**
《Axios》報導稱，美國聯邦眾議員辛克及多位參議員致函ITC，要求對台積電專利侵權案嚴格執法，不應給予特殊待遇
[連結](https://tw.news.yahoo.com/台積電涉專利侵權-多位共和黨議員要求嚴格執法-不應有特殊待遇-044042907.html)

**台積電捲入美ITC專利案，AI加速器先進製程晶片面臨進口禁令風險**
【財訊快報／陳孟朔】全球晶圓代工一哥台積電(2330)在美國面臨專利侵權申訴，《Axios》報導兩家總部位於愛爾蘭都柏林的專利授權公司提出告訴
[連結](https://tw.stock.yahoo.com/news/台積電捲入美itc專利案-ai加速器先進製程晶片面臨進口禁令風險-043651401.html)

**高股息ETF成資金避風港 公股最愛10檔**
在大盤下跌期間，高股息ETF成為資金避風港，特別受到公股機構青睞，統計顯示公股最愛的10檔高息ETF資金流入顯著
[連結](https://example.com/news/3)

台股整體結論:
今日台股市場表現承壓，主要受美股通膨風險及地緣政治不確定性影響，加權指數盤中震盪跌幅擴大。個股表現分歧，市場觀望情緒濃厚，建議投資人密切關注國際局勢變化，操作宜保守。

---

## 格式規範摘要

| 項目 | 規範 |
|------|------|
| Header | `Cronjob : 每日台灣股市新聞` + `(job_id: 3b19d0669d5a)` + `skills: daily-news-twstock` |
| 標題 | 粗體 `**標題**`（第一行） |
| 摘要 | 一句核心描述（第二行） |
| 連結 | Markdown 超連結 `[連結](網址)`，隱藏網址（第三行） |
| 分隔 | 每筆新聞之間以空行分隔 |
| 筆數 | 固定 10 條 |
| 結論 | 末尾附「台股整體結論」段落 |
| 語言 | 100% 繁體中文，無英文說明 |
| 禁止 | 半形冒號、`來源：`、`出處：`、`重要` 等贅詞 |

## 技術細節
- **腳本路徑**: `/root/.hermes/scripts/daily_tw_stock_news.py`
- **輸出語法**: `f"**{title}**\n{summary}\n[連結]({link})"`
- **RSS 來源**: 純台股（Yahoo 台股、鉅亨網），無 blogwatcher-cli、無美股
- **超時**: 硬超時 180 秒、軟超時 120 秒

## 相關連結
- [[daily-news-sources-rss|每日新聞來源管理清單]]
- [[daily-news-twstock-example|每日台股新聞推送規範（技能）]]
