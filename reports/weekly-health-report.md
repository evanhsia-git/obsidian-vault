---
title: "Weekly Health Report"
description: "Obsidian Vault 每週健康報告"
type: report
status: active
summary: "Obsidian Vault 健康檢查報告 2026-06-21（最終）"
tags: [lint, deploy]
created: 2026-06-21
updated: 2026-06-21
---

# LINT REPORT 2026-06-21（最終）

總頁面數：130（不含 raw/ ivan-notes/ database/）

P0 問題

孤立節點：2 頁（1.5%）
- karpathy-llm-wiki-gist（raw/ 唯讀區）
- schema 3.0（ivan-notes/ 唯讀區）

壞連結：0 處
Frontmatter 缺失：0 處
無效 type 值：0 個
無效 status 值：0 個

P1 問題

Index 缺漏：已修復
命名違規：0

P2 問題

出站連結不足（<2）：11 頁（原本 70 頁，已修復 59 頁）
過期頁面（>90天）：0 頁
大型頁面（>200行）：7 頁（SCHEMA 和 POLICY 不拆分）

本次修復摘要

- 修正 status: published -> active：57 個檔案
- 修正 type 值：6 個檔案
- 修復孤立節點：27 頁加入對應 index
- 修復出站連結不足：59 頁補充 Wikilinks
- 修復 Frontmatter 缺失：4 個檔案
- 更新 concepts/index.md、entities/index.md、skills/index.md、reports/index.md、queries/index.md

Vault Status：HEALTHY
## Quartz Migration

