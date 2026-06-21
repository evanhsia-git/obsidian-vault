---
title: 每日AI及科技新聞輸出範例
description: 每日AI及科技新聞輸出範例 — 實體資料頁面
summary: 每日AI及科技新聞輸出範例
type: entity
status: active
priority: P2
tags: ["ai"]
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

# 📋 每日 AI 及科技新聞輸出範例

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

**諂媚率增 25 倍？AI 記憶系統反讓模型寧可附和、不講真話**
最新研究指出，AI 的記憶與個人化功能會引發「諂媚」傾向，為迎合使用者偏誤而犧牲準確性，在高風險領域構成潛在威脅
[連結](https://www.inside.com.tw/article/41526-ai-memory-systems-amplify-sycophancy-writer-research)

**美超微 390 億美元 AI 訂單在手卻墊不起零件款，70 億美元增資引爆稀釋疑慮**
為支應 390 億美元 AI 伺服器訂單，美超微宣布 70 億美元股權融資計畫，引發市場對股本稀釋及訂單不確定性疑慮，股價重挫
[連結](https://www.inside.com.tw/article/41525-supermicro-7-billion-equity-raise-39-billion-ai-orders)

**Neura Robotics 完成 14 億美元 C 輪募資，NVIDIA、亞馬遜、Tether 齊押注歐洲人形機器人**
德國人形機器人新創 Neura Robotics 完成 C 輪募資上看 14 億美元，資金將用於全球部署與擴大產能
[連結](https://www.inside.com.tw/article/41524-neura-robotics-1-4b-series-c-tether-nvidia-humanoid)

科技整體觀察:
今日 AI 與科技領域動態聚焦於晶片、生成式 AI、人形機器人與供應鏈發展，產業競爭與技術疊代持續加速。

---

## 格式規範摘要

| 項目 | 規範 |
|------|------|
| Header | 已移除 |
| 標題 | 粗體 `**標題**`（第一行） |
| 摘要 | 一句核心描述（第二行） |
| 連結 | Markdown 超連結 `[連結](網址)`，隱藏網址（第三行） |
| 分隔 | 每筆新聞之間以空行分隔 |
| 筆數 | 固定 10 條 |
| 結論 | 末尾附「科技整體觀察」段落 |
| 語言 | 100% 繁體中文，無英文說明 |
| 禁止 | 半形冒號、編號、`來源：`、Markdown 表格 |

## 技術細節
- **腳本路徑**: `/root/.hermes/scripts/daily_tech_news.py`
- **輸出語法**: `f"**{title}**\n{summary}\n[連結]({link})"`
- **純中文 RSS 來源**（無 blogwatcher-cli、無 web_search）：
  - 科技新報：`https://technews.tw/feed/`
  - INSIDE：`https://www.inside.com.tw/feed/rss`
- **AI/科技關鍵字過濾**: 腳本內建 KEYWORDS 篩選（AI、半導體、晶片、量子、模型、機器人、雲端等）
- **已排除來源**: iThome RSS（回傳 2022 舊資料）、鉅亨網 tech RSS（回傳 0 則）
- **超時**: 硬超時 180 秒、軟超時 120 秒

## 相關連結
- [[daily-news-sources-rss|每日新聞來源管理清單]]
- [[daily-news-twstock-example|每日台股新聞輸出範例]]
- [[daily-news-usstock-example|每日美股新聞輸出範例]]
- [[daily-news-stock-market-example|每日股市指標輸出範例]]
- [[daily-news-technology-example|每日AI及科技新聞推送規範（技能）]]
