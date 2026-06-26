---
title: "LLM Wiki 架構與比較分析"
description: "LLM Wiki 架構與比較分析 — 概念說明頁面"
summary: "LLM Wiki 架構與比較分析"
type: concept
status: active
tags: [obsidian, flow, hermes]
created: 2026-06-05
updated: 2026-06-05
---

# LLM Wiki 架構與比較分析

本頁面記錄 Hermes-Agent 所採用的 LLM Wiki 模式與市面上主流 GitHub 專案的差異與優勢分析。

## 核心定位

Hermes-Agent 的 Wiki 實現並非單一開源軟體，而是基於 [Andrej Karpathy LLM Wiki 概念](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 進行高度客製化的 **Agent-Centric 知識架構**。

## 比較分析

| 特性 | Hermes 原生版 (本系統) | domleca/llm-wiki | nvk/llm-wiki |
| :--- | :--- | :--- | :--- |
| **定位** | **Agent-Centric** (為代理設計) | 個人筆記管理工具 | 純文字知識庫框架 |
| **核心優勢** | 強制前置導航 (SCHEMA→index→log)、與 Agent 工具鏈深度整合 | 介面美觀，適合人類閱讀 | 簡單輕量，易於部署 |
| **自動化程度** | 極高 (Agent 自動維護與 Lint) | 中等 (需手動維護) | 低 (靜態導向) |
| **顯示限制** | 強制 Unicode 符號，Telegram 相容 | 不設限制 | 不設限制 |
| **執行流程** | 嚴格遵守「導航→提取→執行→沉澱」 | 無 | 無 |

## 本系統 (Hermes 原生版) 之關鍵技術特色

### 1. 強制導航協議 (Navigation Protocol)
不同於一般筆記系統，本架構要求 Agent 在每次對話前必須執行 `SCHEMA` → `index` → `log` 導航，確保 Agent 在處理請求前已讀取正確的上下文脈絡。

### 2. 閉環自動化 (Self-Maintenance)
- **Lint Procedure**：具備自動健康檢查機制，可檢測孤立頁面、斷鏈、LaTeX 符號洩漏與 Frontmatter 格式。
- **知識沉澱 (Ingest & Compound)**：任務結束後，自動將價值結論更新至 Wiki 並同步 `log.md`。

### 3. Telegram 生態優化
針對即時通訊平台顯示限制進行極端優化：
- **LaTeX 禁用**：全面改用 Unicode 符號（如 →, ⇒, ±, ×），確保在 Telegram 的顯示準確度。
- **交付規範**：明確區分 Markdown 連結與純文字路徑引用。

## 結論
我們的 LLM Wiki 不僅是筆記，更是 **「代理人的外部大腦」**。我們追求的是與 Hermes Agent 工具鏈的極致整合，而非單純的知識儲存。此架構具備自動化能力，能隨 Agent 的執行過程自我成長，是 Agent 高效運作的基石。

相關頁面：[[llm-wiki-concept]]

相關頁面：## 相關節點
- [[index]]
