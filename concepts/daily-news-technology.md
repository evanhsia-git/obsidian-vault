---
status: active
title: "daily-news-technology"
description: "每日 AI/科技新聞推送技能"
summary: "daily-news-technology：每日AI及科技新聞 (新格式)"
created: 2026-06-10
updated: 2026-06-10
type: concept
tags: [hermes]
---

# 每日AI及科技新聞

## 每日AI及科技新聞 (新格式)

**【聯準會維持利率不變】**：美聯儲於6月18日維持利率不變，市場持樂觀，預期將支持股市持續上漲。: https://example.com/fed-rate

- [[openrouter-free-models]]
**【科技股表現強勁】**：納斯達克指數連續五日上漲，人工智慧相關股票領漲，英偉達漲幅超過5%，成為市場領先股。: https://example.com/tech-stock

**【蘋果營收創歷史新高】**：蘋果第一季營收創歷史新高，雲端業務顯著成長，股價盤後上漲。: https://example.com/apple-earnings

**【美股指數全線上漲】**：三大指數全線上漲，科技股領漲，市場對人工智慧與雲端計算持續看好，投資者情緒偏多。: https://example.com/us-index

**【非農就業數據強勁】**：美國非農就業人數超預期，失業率維持低位，經濟基本面強勁，支撐股市表現。: https://example.com/nonfarm

**【歐洲央行維持利率不變】**：歐洲央行維持利率不變，市場預期美國將率先降息，美元走弱利好美股。: https://example.com/ecb-rate

**【大型科技企業收購】**：大型科技企業宣佈收購計畫，市場對整合效應反應積極，相關股價上漲。: https://example.com/tech-acquisition

**【亞洲與歐洲股市穩健】**：亞洲市場表現穩健，歐洲小幅上漲，全球股市呈上漲趨勢。: https://example.com/asia-eu-market

**【新能源車股表現亮眼】**：新能源車銷售數據強勁，相關股價持續上漲。: https://example.com/ev-stocks


## 相關節點
- [[index]]

## Daily News Stock Market

---
title: "README"
description: "README — 技能說明頁面"
summary: "README"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

# Daily News Stock Market Skill 知識庫

## 概述
- [[skills/skills-index]]
本頁面記錄 `daily-news-stock-market` 技能的相關規格、知識與維護紀錄。

## 執行規範 (SCHEMA → index → log)
1. **導航**: 確認當日指標是否已抓取。
2. **提取**: 透過 `daily-news-stock-market` skill 中的腳本自動抓取數據。
3. **執行**: 運行 cronjob `a0144cdf0461`。
4. **沉澱**: 將當日報告結果寫入 `/log/market-reports/{{date}}.md`。

## 修正內容紀錄
- **2026-06-10**:
  - 修正 KOSPI 數據抓取邏輯 (Naver Finance ID)。
  - 修正 CNN Fear & Greed Index 解析邏輯 (正則匹配)。
  - 更新報告輸出格式 (加入 `skills: daily-news-stock-market` 標籤)。

## 參考資料
- [相關報告模板](/templates/report-template.md)
- [Cron 輸出規則](/references/cron-output-rules.md)


## 相關節點
- [[index]]

## Daily News Stock Market

本頁面記錄 `daily-news-stock-market` 技能的相關規格、知識與維護紀錄。

## 執行規範 (SCHEMA → index → log)
1. **導航**: 確認當日指標是否已抓取。
2. **提取**: 透過 `daily-news-stock-market` skill 中的腳本自動抓取數據。
3. **執行**: 運行 cronjob `a0144cdf0461`。
4. **沉澱**: 將當日報告結果寫入 `/log/market-reports/{{date}}.md`。

## 修正內容紀錄
- **2026-06-10**:
  - 修正 KOSPI 數據抓取邏輯 (Naver Finance ID)。
  - 修正 CNN Fear & Greed Index 解析邏輯 (正則匹配)。
  - 更新報告輸出格式 (加入 `skills: daily-news-stock-market` 標籤)。

## 參考資料
- [相關報告模板](/templates/report-template.md)
- [Cron 輸出規則](/references/cron-output-rules.md)


## 相關節點
- [[index]]
## Daily News TW Stock

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

- [[cnn-fear-and-greed-analysis]]
## 相關連結
- [[daily-news-sources-rss|每日新聞來源管理清單]]
- [[news-and-market-examples|每日台股新聞推送規範]]