---
title: README
description: README — 技能說明頁面
summary: README
type: concept
status: published
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

# Daily News Stock Market Skill 知識庫

## 概述
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
