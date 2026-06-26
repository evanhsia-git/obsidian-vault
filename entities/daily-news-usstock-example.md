---
title: "每日美股新聞輸出範例"
description: "每日美股新聞輸出範例 — 實體資料頁面"
summary: "每日美股新聞輸出範例"
type: entity
status: active
tags: []
created: 2026-06-11
updated: 2026-06-11
---

# 📋 每日美股新聞輸出範例

## Header 格式
```
Cronjob : 每日美國股市新聞
(job_id: a7e796ca66c0)
skills: daily-news-usstock
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

Cronjob : 每日美國股市新聞
(job_id: a7e796ca66c0)
skills: daily-news-usstock

**30年未見！瑞銀分析師：半導體設備商能見度看到2028年 營收上看2500億美元**
瑞銀分析師指出，隨著美光、三星與SK海力士等記憶體大廠啟用新晶圓廠，全球半導體設備市場正邁入超級周期早期階段，預估至2028年相關營收上看2500億美元
[連結](https://news.cnyes.com/news/id/6495363)

**價格戰開打在即？WSJ：OpenAI考慮大幅降價 預計將與Anthropic展開用戶爭奪戰**
《華爾街日報》報導，AI龍頭OpenAI正考慮大幅調降服務收費，劍指勁敵Anthropic，矽谷AI軍備競賽蔓延至價格戰
[連結](https://news.cnyes.com/news/id/6495371)

**川普下令重襲伊朗 中東警報大響 科威特攔截空中目標、荷姆茲海峽全面關閉**
美軍對伊朗多處軍事設施發動新一輪空襲後，中東局勢迅速惡化，伊朗宣布全面封鎖荷姆茲海峽並威脅攻擊通行船隻，推升國際油價續漲
[連結](https://news.cnyes.com/news/id/6495342)

美股整體結論:
美股近期受通膨數據與地緣政治情勢牽動，主要指數震盪，科技股與AI類股波動加劇，市場觀望情緒升溫，投資人宜留意聯準會政策動向與國際局勢變化。

---

## 格式規範摘要

| 項目 | 規範 |
|------|------|
| Header | `Cronjob : 每日美國股市新聞` + `(job_id: a7e796ca66c0)` + `skills: daily-news-usstock` |
| 標題 | 粗體 `**標題**`（第一行） |
| 摘要 | 一句核心描述（第二行） |
| 連結 | Markdown 超連結 `[連結](網址)`，隱藏網址（第三行） |
| 分隔 | 每筆新聞之間以空行分隔 |
| 筆數 | 固定 10 條 |
| 結論 | 末尾附「美股整體結論」段落 |
| 語言 | 100% 繁體中文，無英文說明 |
| 禁止 | 半形冒號、編號、`來源：`、`出處：`、Markdown 表格 |

## 技術細節
- **腳本路徑**: `/root/.hermes/scripts/daily_us_stock_news.py`
- **輸出語法**: `f"**{title}**\n{summary}\n[連結]({link})"`
- **純中文 RSS 來源**（無 blogwatcher-cli、無英文來源）：
  - Yahoo 美股：`https://tw.stock.yahoo.com/rss?category=us-market`
  - 鉅亨網全球股市：`https://news.cnyes.com/rss/v1/news/category/wd_stock`
- **超時**: 硬超時 180 秒、軟超時 120 秒

## 相關連結
- [[daily-news-sources-rss|每日新聞來源管理清單]]
- [[daily-news-twstock-example|每日台股新聞輸出範例]]
- [[daily-news-usstock-example|每日美股新聞推送規範（技能）]]
