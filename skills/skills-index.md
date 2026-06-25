---
title: "Skills 目錄"
description: "Skills 目錄 — 索引頁面"
summary: "Skills 目錄索引"
type: index
status: active
tags: [agent]
created: 2026-06-23
updated: 2026-06-23
---

# Skills 目錄

## 索引與架構

- [[skills-list|Skills List Display]] — 技能清單顯示
- [[skills-schema|Schema]] — Skills 架構
- [[blogwatcher-index|Blogwatcher Index]] — 部落格監測索引

## 安裝與部署

- [[superpowers-install|Superpowers 安裝說明]] — obra/superpowers 14 skills 匯入流程
- [[superpowers-reference|Superpowers 功能說明]] — 14 個 skills 完整功能說明與工作流程
- [[code-quality-analysis|程式碼品質分析]] — 效能瓶頸、重複程式碼、過長函式、不必要檔案存取分析與優化方案

## 技能與架構

- [[skill-usage-protocol|Skill 使用規範]] — 新建技能前必須先檢視技能清單
- [[skill-script-architecture|技能腳本架構管理]] — scripts/ vs skills/ 架構決策
- [[obsidian-wiki-skill|Obsidian Wiki 技能 v3.0]] — 導航 + 安全規則
- [[python-in-skill-implementation|Python in Skill Implementation]] — Python 在 skill 中的優勢
- [[cron-architecture-roles|Cron 架構角色分工]] — Skills、Python、no_agent 角色
- [[manus-use-cases|Manus 精選案例與實際應用]] — 6 類 Manus 案例
- [[blave-quant-skill|Blave Quant Skill]] — 加密貨幣交易技能包

## 故障排除

- [[troubleshooting/hermes-evolution|Hermes Evolution]] — Hermes 演化記錄
- [[troubleshooting/skill-archiving-sop|Skill 歸檔封存 SOP]] — 技能歸檔標準作業程序

## 任務管理

- [[user-backup-skill|User Backup Skill]] — 每日系統維護編排器
- [[daily-news-stock-market-index|每日市場指標報告]]

## 使用狀況

### 整體統計

| 指標 | 數值 |
|------|------|
| SKILL.md 總數 | 199 |
| User 自建 | 50 |
| Superpowers | 14 |
| Productivity | 20 |
| Software Dev | 18 |
| Note Taking | 8 |
| DevOps | 9 |
| Finance | 5 |
| AI | 1 |
| Hermes News | 1 |
| Apple | 2 |

### 新聞推送系列

| Skill | 狀態 | 綁定 Cron | 最後活動 | 說明 |
|-------|------|-----------|---------|------|
| daily-news-unified | active | 無（手動觸發） | — | 頂層聚合器，引用 4 個 domain skills |
| daily-news-technology | active | e2532c7ccbda | 0d | AI/科技新聞 10 則，09:00/17:00 |
| daily-news-twstock | active | 941d5aaa9119 | 0d | 台股新聞 10 則，08:30/15:00 |
| daily-news-usstock | active | 8bc577c85cad | 0d | 美股新聞 10 則，08:30/22:00 |
| daily-stock-news | active | ca7b49e89df2 | 0d | 亞洲指數+美國指標+CNN+宏觀，08:30 |
| daily-news-stock-market | **DEPRECATED** | — | — | 已合併至 daily-stock-news（2026-06-25） |

### 整併歷史

| 日期 | 動作 | 說明 |
|------|------|------|
| 2026-06-25 | 整併 | `daily-news-stock-market` → `daily-stock-news`；`daily-news-unified` 更新為頂層聚合器 |

### 零活動 skills（Curator 優先整併候選）

以下 6 個 skill 從未被使用過（activity=0, use=0, view=0, patches=0, last_activity=never）：

- apple-notes
- apple-reminders
- backtest-visualizer
- cnn-fear-and-greed-api
- findmy
- system-capability-audit

### Curator 設定

```yaml
curator:
  enabled: true
  interval_hours: 168
  min_idle_hours: 2
  stale_after_days: 30
  archive_after_days: 90
  consolidate: false
  prune_builtins: true
```
