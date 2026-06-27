---
title: "Blogwatcher 技能知識庫"
description: "Blogwatcher CLI 工具與 RSS 監控機制的完整說明"
summary: "Blogwatcher 效益分析 + 執行流程 + RSS 來源清單 + 維護原則"
type: index
status: active
tags: [hermes, news, workflow]
created: 2026-06-10
updated: 2026-06-27
---

# Blogwatcher 工具與 RSS 監控機制

## 摘要

Blogwatcher 是 Hermes Agent 內建的終端機監控工具 (`blogwatcher-cli`)，用於監控部落格與 RSS/Atom feeds。無需額外部署 Miniflux 或 Docker，直接在 VPS 上透過腳本運行。

## 核心效益

| 評估項目 | 原本方式 (web_extract) | Blogwatcher (推薦) |
| :--- | :--- | :--- |
| 執行效率 | 低（需搜尋+解析網頁） | 極高（僅抓取 XML 更新） |
| Token 消耗 | 高（含廣告雜訊） | 極低（純文字高密度） |
| 穩定性 | 易受網站改版干擾 | 高（標準 RSS 協定） |

## 執行流程

1. **導航**：確認目標網站是否有 RSS/Atom Feed
2. **提取**：`blogwatcher-cli` 訂閱並同步內容
3. **執行**：Agent 定時讀取本地監控紀錄
4. **沉澱**：內容標準化後寫入 log

## RSS 監控來源

### 台灣
- 鉅亨網: `https://news.cnyes.com/rss/news/cat/tw_stock`
- iThome: `https://www.ithome.com.tw/rss`
- INSIDE: `https://www.inside.com.tw/feed`

### 美國
- TechCrunch: `https://techcrunch.com/feed/`
- Bloomberg: `https://feeds.bloomberg.com/technology/news.rss`
- Wired: `https://www.wired.com/feed/rss`

### 中國
- 機器之心: `https://www.jiqizhixin.com/rss`
- 36氪: `https://36kr.com/feed`
- 虎嗅網: `https://www.huxiu.com/rss/0.xml`

## 維護原則

- **降噪**：優先使用 Blogwatcher 標準化解析
- **一致性**：所有 RSS 來源統一加入 `blogwatcher-cli` 訂閱清單
- **更新**：新增來源後執行 `blogwatcher-cli update`

## 相關節點

- [[news-sources]]
- [[daily-news-technology-example]]
- [[daily-news-twstock-example]]
