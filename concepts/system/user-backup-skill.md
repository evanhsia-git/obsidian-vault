---
title: User Backup Skill
description: 每日系統維護編排器
type: concept
tags: [hermes, backup, maintenance]
summary: user-backup 系統級自動化維護 Orchestrator
created: 2026-06-21
updated: 2026-06-21
---

# user-backup（每日系統維護編排器）

## 📌 定義
`user-backup` 是一個系統級的自動化維護 Orchestrator，負責整合 Lint 檢查、多路 Git 備份、容量監控與 Quartz 網站部署。它確保了從「筆記撰寫」到「網站發佈」整個生命週期的完整性與安全性。

**核心原則：VPS 本機是主資料，GitHub 是備份區。任何同步衝突以本機為準。**

## 🚀 執行流程（Workflow）

### 階段零：Obsidian Lint 檢查
