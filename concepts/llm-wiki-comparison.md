---
title: "LLM Wiki 架構與比較分析"
description: "LLM Wiki 與 Karpathy 概念、domleca/nvak 的比較"
summary: "LLM Wiki 比較表 + Karpathy 標籤池分析結論"
type: concept
status: active
tags: [ai, pkm, hermes]
created: 2026-06-05
updated: 2026-06-27
---

# LLM Wiki 架構與比較分析

## 比較таблиця

| 特性 | Hermes 原生 | domleca/llm-wiki | nvk/llm-wiki |
|:---|:---|:---|:---|
| 定位 | Agent-Centric | 個人筆記 | 純文字框架 |
| 自動化 | 極高（Lint + 自動沉澱） | 中（手動） | 低 |
| 導航 | SCHEMA→index→log 強制 | 無 | 無 |
| 顯示 | Unicode only， Telegram 相容 | 不設限 | 不設限 |
| 知識累積 | 持久複利（ Wiki = 程式碼庫） | 單向組織 | 靜態 |

## Karpathy 核心概念

- **關鍵句**：「Wiki 是一个持久、複利的產物。」
- **三層架構**：Raw（不可變）→ Wiki（Agent 維護）→ Schema（規範）
- **核心流程**：Ingest（攝取→入 raw/ + frontmatter）→ Query（讀 index→綜合→存 queries/）→ Lint（孤立頁/斷鏈/過期/矛盾）
- **Self-Contained 原則**：每個知識單元自給自足

## 標籤池分析結論

**問題**：標籤同時扮演「分類導航」（人類）和「RAG 過濾」（Agent）兩種衝突角色

**建議（不擴張池子）**：
1. 補 10–12 個精準缺口標籤
2. `summary` 升格必填
3. 系統維護類標籤（index/log/maintenance）改為 type 欄位控制
