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

# Blogwatcher 監控技能知識庫

## 1. 摘要：什麼是 Blogwatcher？
Blogwatcher 是一款 Hermes Agent 內建的終端機監控工具 (blogwatcher-cli)，專門用於監控部落格與 RSS/Atom feeds。相較於外部服務（如 Miniflux），它完全運行於 Agent 原生環境，具備極高的整合性與維護便利性。

## 2. 核心效益分析
| 評估項目 | 原本方式 (web_extract) | Blogwatcher (推薦方案) |
| :--- | :--- | :--- |
| **執行效率** | 低 (每次需搜尋與完整解析網頁) | **極高** (僅抓取變更的摘要內容) |
| **Token 消耗** | 高 (包含廣告、導覽列等雜訊) | **極低** (僅純文字內容，高密度) |
| **維護複雜度** | 低 (無額外服務) | **極低** (Agent 原生指令) |
| **穩定性** | 易受網站改版干擾 | **高** (基於標準 RSS 協定) |

## 3. 執行流程 (SCHEMA → index → log)
1. **導航**: 定期檢查 `skills/blogwatcher/` 下的監控清單與執行日誌。
2. **提取**: 使用 `blogwatcher-cli` 監控指定 Feed，自動抓取變更內容。
3. **執行**: 處理變更內容並依照新聞格式轉譯 (標準化輸出)。
4. **沉澱**: 將整理後的新聞更新至 Obsidian Vault 的 `log/market-reports/` 或相關目錄。


## 相關節點
- [[index]]
