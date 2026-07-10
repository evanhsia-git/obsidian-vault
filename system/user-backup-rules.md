---
status: active
title: "User Backup Rules"
summary: "User Backup Rules - 備份規範與每日系統維護編排器（合併 backup-rules + user-backup-skill）"
description: "備份、同步、還原規範；整合 Lint 檢查、多路 Git 備份、容量監控與 Quartz 部署的 user-backup Orchestrator"
type: concept
tags: [backup, sync, workflow, deploy, hermes]
created: 2026-06-21
updated: 2026-07-10
---

# User Backup Rules

> 本檔於 2026-07-10 合併原 `system/backup-rules.md`（備份規範）與 `skills/user-backup-skill.md`（技能定義）為單一來源，舊兩檔已刪除。

## 📌 定義

`user-backup` 是一個系統級的自動化維護 Orchestrator，負責整合 Lint 檢查、多路 Git 備份、容量監控與 Quartz 網站部署。它確保了從「筆記撰寫」到「網站發佈」整個生命週期的完整性與安全性。

**核心原則：VPS 本機是主資料，GitHub 是備份區。任何同步衝突以本機為準。**

## 🔧 備份策略

| 類型 | 工具 | 頻率 |
|:--|:--|:--|
| Git Backup | git push | 每次變更 |
| Database Backup | sqlite3 .dump | 每日 |
| Snapshot | rsync | 每日 |
| Full Backup | tar + gzip | 每週 |

## 🔒 Git 備份原則

- VPS 本機是主資料，GitHub 是備份區
- push 失敗時：pull rebase 若衝突則 abort → force push（以本機為準）
- 重要操作前：`git add -A && git commit -m "pre-op"`

## 🚀 執行流程（Workflow）

1. **階段零：Obsidian Lint 檢查** — 對 Vault 跑 lint，修正孤兒/格式問題
2. **階段一：多路 Git 備份** — Obsidian Vault、Quartz content、Hermes 配置分別 commit + push
3. **階段二：容量監控** — 檢查磁碟使用率，超門檻告警
4. **階段三：Quartz 部署** — 重建並推送靜態網站

對應腳本：`/root/.hermes/scripts/user-backup.sh`（由 cron job `545d8fb6a9e8` 每日 05:00 執行）。

## 🔄 還原原則

- 先備份再還原
- 還原後驗證資料完整性
- 重大還原需取得使用者核准

## 🛡️ 安全規範

- 備份檔案禁止存放在 `raw/`
- 敏感檔案（.env、API Key）禁止備份到 Git
- `ivan-notes/`、`database/`、`copilot/` 已 Git-ignored，不進版本控制

## 📊 最近執行結果

<!-- EXECUTION_LOG_START -->
（待執行 user-backup 後寫入）
<!-- EXECUTION_LOG_END -->

## 相關連結

- [[schema]]：核心憲法
- [[policy]]：規則路由器
- [[folder-rules]]：目錄結構與讀寫權限
