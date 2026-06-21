---
title: 技能腳本架構管理（Skill Script Architecture）
description: 探討 Python 腳本應放在 /scripts/ 還是 skills/ 目錄下的優缺點分析
summary: 技能腳本放置位置的架構決策指南
type: concept
status: active
priority: P2
tags: [system, skill, setup, maintenance]
aliases: [腳本位置, 技能架構]
created: 2026-06-16
updated: 2026-06-16
date: 2026-06-16
---

# 技能腳本架構管理

## 問題

Python 腳本目前放在 `/root/.hermes/scripts/` 底下，但 Hermes 技能目錄為 `/root/.hermes/skills/user/<skill_name>/`。是否應將腳本遷移到技能目錄下？

## 目前狀態

| 技能 | SKILL.md 位置 | Python 腳本位置 |
|------|--------------|----------------|
| daily-news-twstock | `skills/user/daily-news-twstock/SKILL.md` | `scripts/daily-news-twstock.py` |
| daily-news-technology | `skills/user/daily-news-technology/SKILL.md` | `scripts/daily-news-technology.py` |
| daily-news-usstock | `skills/user/daily-news-usstock/SKILL.md` | `scripts/daily-news-usstock.py` |
| daily-news-stock-market | `skills/user/daily-news-stock-market/SKILL.md` | `scripts/daily-news-stock-market.py` |

## 方案比較

### 方案 A：留在 `/root/.hermes/scripts/`（目前）

**優點**：
- 路徑簡短，呼叫方便
- 所有腳本集中管理，容易全域搜尋
- 遷移成本低

**缺點**：
- SKILL.md 與腳本分離，管理分散
- 技能數量增多時 `/scripts/` 會變得雜亂
- 不符合 Hermes 技能模組化設計理念

### 方案 B：移到 `skills/user/<skill_name>/scripts/`（建議）

**優點**：
- **高內聚性**：SKILL.md、腳本、設定檔都在同一目錄
- **模組化**：每個技能自成一體，方便備份、刪除、移植
- **擴展性**：可在技能目錄下建立 `data/`、`templates/` 等子目錄
- **符合規範**：遵循 Hermes 技能設計模式

**缺點**：
- 路徑變長
- 需要更新所有 Cron Job 的 script 路徑
- 遷移時需要全面測試

## 決策建議

| 情境 | 建議 |
|------|------|
| 技能數量少（< 5）且邏輯簡單 | 留在 `/scripts/` |
| 技能數量多（> 10）或有配套文件 | 移到 `skills/` |
| 有複雜資料夾結構（含 CSV/JSON） | 移到 `skills/` |

## 遷移步驟（如決定執行）

1. 建立目錄：`mkdir -p /root/.hermes/skills/user/<skill_name>/scripts/`
2. 移動檔案：`mv /root/.hermes/scripts/<script>.py <新路徑>`
3. **更新 Cron Job**：修改所有對應的 `script` 路徑（最關鍵）
4. 更新 SKILL.md 中的路徑記錄
5. 執行測試確認

## 相關頁面

- [[daily-news-sources-rss|每日新聞來源管理清單]]
- [[stock-automation-config|Cron Job 管理]]
