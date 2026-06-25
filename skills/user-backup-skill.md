---
title: "User Backup Skill"
description: "每日系統維護編排器，整合 Lint 檢查、Git 備份、容量監控與 Quartz 部署"
summary: "user-backup 系統級自動化維護 Orchestrator"
type: index
status: active
tags: [hermes, backup, deploy]
related: "[[skills/skills-index]]"
created: 2026-06-21
updated: 2026-06-25
---

# user-backup（每日系統維護編排器）

## 📌 定義
`user-backup` 是一個系統級的自動化維護 Orchestrator，負責整合 Lint 檢查、多路 Git 備份、容量監控與 Quartz 網站部署。它確保了從「筆記撰寫」到「網站發佈」整個生命週期的完整性與安全性。

**核心原則：VPS 本機是主資料，GitHub 是備份區。任何同步衝突以本機為準。**

## 🚀 執行流程（Workflow）

### 階段零：Obsidian Lint 檢查

## 相關節點
- [[schema]]
- [[skills/obsidian-wiki-skill|Obsidian Wiki 技能]]
- [[skills/skill-usage-protocol|Skill 使用規範]]
- [[skills/cron-architecture-roles|Cron 架構角色分工]]
- [[skills/skills-index|Skills 目錄]]
