---
title: "Curator 技能管理員"
description: "Hermes Agent 的 Curator 背景維護機制，負責 skill 生命週期管理"
summary: "Curator：Hermes 自動化管理 skill 生命週期（active → stale → archived），避免重複/無用 skill 堆積"
type: concept
status: active
tags: [hermes, agent, deploy]
created: 2026-06-25
updated: 2026-06-25
---

# Curator 技能管理員

## 概述

Curator 是 Hermes Agent 的**背景維護行程**，專門管理 agent 自行創建的 skill 生命週期。它追蹤每個 skill 的查看、使用、修補次數，將長期未使用的 skill 從 `active → stale → archived` 狀態遷移，並週期性觸發輔助模型審查，提出整併或修補建議。

**存在目的**：防止 self-improvement loop 無限堆積狹義近重複 skill，避免目錄污染和 token 浪費。

## 核心機制

### 觸發方式

- **非 cron daemon**，由閒置檢查觸發
- CLI session 啟動時 + gateway cron-ticker thread 週期檢查
- 執行條件：`interval_hours`（默認 168h/7天）且 `min_idle_hours`（默認 2h）

### 生命週期

```
active → stale → archived
```

- **stale**：超過 `stale_after_days`（默認 30 天）未使用
- **archived**：超過 `archive_after_days`（默認 90 天）未使用
- 存檔位置：`~/.hermes/skills/.archive/`（可恢復，**永不自動刪除**）

### 技能分類與保護

| 類別 | Curator 行為 | 說明 |
|------|-------------|------|
| Agent-created skill | 主要管理對象 | `created_by: agent` 標記 |
| Bundled built-in skill | 可歸檔（默認 `prune_builtins: true`） | 可設 `false` 排除 |
| Hub-installed skill（agentskills.io） | **永遠豁免** | 不碰 |
| User-directed skill | **不管理** | 前景 agent 手動建立/手動建立的 skill |

## 兩大階段

### 1. Pruning（確定性）

- 標記 stale、歸檔長期未使用 skill
- 默認行為（無需額外 token）

### 2. Consolidation（LLM 輔助）

- 默認**關閉**（`consolidate: false`）
- 需手動啟用 `curator.consolidate: true` 或 `hermes curator run --consolidate`
- 消耗輔助模型 token，進行結構性整併

## 配置（config.yaml）

```yaml
curator:
  enabled: true
  interval_hours: 168        # 7 天
  min_idle_hours: 2
  stale_after_days: 30
  archive_after_days: 90
  consolidate: false         # LLM 整併（默認關閉）
  prune_builtins: true       # 歸檔未使用的 built-in skill
```

### 輔助模型設定

- 默認使用主聊天模型
- 可透過 `auxiliary.curator` 或 `hermes model` 指定便宜模型（如 `google/gemini-3-flash-preview`）
- 舊格式 `curator.auxiliary.{provider,model}` 仍支援但會輸出 deprecation log

## CLI 指令

### 狀態查詢

- `hermes curator status`：上次運行時間、計數、pinned 列表、LRU skill
- `hermes curator list-archived`：列出已歸檔 skill

- 手動歸檔：`hermes curator archive <skill>`
- 手動恢復：`hermes curator restore <skill>`
- 批量修剪：`hermes curator prune [--days N]`

### 備份與回滾

- `hermes curator backup`：手動快照 `~/.hermes/skills/`
- `hermes curator rollback`：恢復最新快照（需確認）
- `hermes curator rollback --list`：列出可用快照
- `hermes curator rollback --id <ts>`：恢復特定快照

### 保護機制

- `hermes curator pin <skill>`：防止自動遷移 + 防止 `skill_manage` 刪除
- `hermes curator unpin <skill>`：取消保護

## 安全特性

1. **永不自動刪除**：最差結果就是歸檔到 `.archive/`，可恢復
2. **首次延遲**：新安裝或 `hermes update` 後，首次運行延遲一個 `interval_hours`
3. **Dry run**：`hermes curator run --dry-run` 可預覽動作不實際執行
4. **背景隔離**：fork 一個 `AIAgent` 背景行程，使用獨立 prompt cache，不影響前景對話

## 與我們系統的关系

目前我們的 skill 多數是**手動建立**（user-directed），因此 Curator 不會自動管理它們。但 Curator 對 self-improvement loop 自動生成的 skill 特別重要：它確保系統不會因為反覆嘗試新做法而無限堆積重複 skill。

若啟用 `consolidate: true`，Curator 會自動偵測近重複 skill 並建議整併，有助於保持 skill 目錄整潔。

## 整併記錄

### 2026-06-25：daily-news 系列整併

**觸發**：Curator LLM 分析發現 7 個 daily-news skill 高度重疊

**執行動作**：
- `daily-news-stock-market` → **合併至** `daily-news-stock-news`
  - 市場數據抓取邏輯（yfinance + FRED + CNN）已整合
  - 輸出格式 header 統一為 `skills: daily-stock-news`
  - Cron job `ca7b49e89df2` 綁定標籤更新
  - 原 skill 標記為 DEPRECATED
- `daily-news-unified` 更新為**頂層聚合器**
  - 明確引用 4 個 domain skills（technology、twstock、usstock、stock-news）
  - 新增市場指數數據域

**合併後結構**：
```
daily-news-unified          ← 頂層聚合器
├── daily-news-technology   ← AI/科技新聞
├── daily-news-twstock     ← 台股新聞
├── daily-news-usstock     ← 美股新聞
└── daily-stock-news       ← 市場指數+數據（吸收 stock-market）
```
