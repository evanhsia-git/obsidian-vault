---
title: "AI Toolkit — 模型、框架與繪圖工具"
description: "AI 相關工具與框架比較：LLM Wiki 架構、Superpowers 插件系統、Next AI Draw.io 繪圖"
summary: "LLM Wiki 比較表 + Superpowers 框架 + Next AI Draw.io — AI 工具鏈知識整合"
type: concept
status: active
tags: [ai, pkm, hermes, workflow]
created: 2026-06-28
updated: 2026-06-28
---

# AI Toolkit — 模型、框架與繪圖工具

以下為 AI 輔助工具與框架的整合知識，涵盖知識管理比較、Agent 外掛架構、以及繪圖自動化方案。

---

## LLM Wiki 架構比較

### 三种方案對比

| 特性 | Hermes 原生 | domleca/llm-wiki | nvk/llm-wiki |
|:---|:---|:---|:---|
| 定位 | Agent-Centric | 個人筆記 | 純文字框架 |
| 自動化 | 極高（Lint + 自動沉澱） | 中（手動） | 低 |
| 導航 | SCHEMA→index→log 強制 | 無 | 無 |
| 顯示 | Unicode only，Telegram 相容 | 不設限 | 不設限 |
| 知識累積 | 持久複利 | 單向組織 | 靜態 |

### Karpathy 核心概念

- **關鍵句**：「Wiki 是一个持久、複利的產物。」
- **三層架構**：Raw（不可變）→ Wiki（Agent 維護）→ Schema（規範）
- **核心流程**：Ingest → Query → Lint
- **Self-Contained 原則**：每個知識單元自給自足

### 標籤池分析結論

標籤同時扮演「分類導航」（人類）和「RAG 過濾」（Agent）兩種衝突角色。建議：補精準缺口標籤、`summary` 升格必填、系統維護類標籤改為 type 欄位控制。

---

## Superpowers 參考框架

[obra/superpowers](https://github.com/obra/superpowers) 是 Agentic 技能與軟體架構框架，特點：

- **技能框架**：Agent 能力模組化，支援跨平台執行（Claude Code, Codex, Copilot CLI）
- **多 Agent 協作**：子代理派發 + 自動化 review
- **可靠生命週期管理**：Brainstorm Server 解決 PID 監控與 EPERM

對 Hermes 的啟發：

| 機制 | 應用場景 | 效益 |
|:---|:---|:---|
| Inline Self-Review | 每日新聞分析、自動編程 | 縮短 subagent review 時間 |
| Owner-PID Monitoring | 長期 Cronjob（RSS 監控） | 防止權限錯誤導致程序終止 |
| Worktree Support | 多專案開發測試 | 隔離環境執行自動化實驗 |

---

## Next AI Draw.io

基於 Next.js + draw.io 的 AI 繪圖工具（[GitHub](https://github.com/DayuanJiang/next-ai-draw-io)）。

- **自然語言繪圖**：透過自然語言建立圖表
- **AI 輔助優化**：自動視覺化、修改與優化
- **應用場景**：快速原型設計、自動化教學、團隊協作優化

---

## 相關頁面

- [[concepts/hermes-hierarchy]]
- [[投資大師選股策略-投資策略investment-strategy]]
- [[concepts/concepts-index]]
