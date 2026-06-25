---
title: "index"
description: "index — 索引頁面"
summary: "index"
type: concept
status: active
priority: P2
tags: [hermes]
aliases: []
created: 2026-06-10
updated: 2026-06-10
date: 2026-06-10
publish: true
draft: false
related:
source:
due:
review:
---

# 每日市場指標報告

## 任務目標
每日產生簡潔的市場指標快訊，覆蓋亞洲指數、美國指標、宏觀經濟及匯率。

## 執行流程 (SCHEMA → index → log)
1. **導航**: 確認當日指標是否已抓取。
2. **提取**: 透過 `daily-news-stock-market` skill 中的腳本自動抓取數據。
3. **執行**: 運行 cronjob `a0144cdf0461`。
4. **沉澱**: 將當日報告結果寫入 `/log/market-reports/{{date}}.md`。

## 最近更新 (2026-06-10)
- 修正 KOSPI 數據抓取邏輯 (Naver Finance ID)。
- 修正 CNN Fear & Greed Index 解析邏輯 (正則匹配)。
- 更新報告輸出格式 (加入 `skills: daily-news-stock-market` 標籤)。