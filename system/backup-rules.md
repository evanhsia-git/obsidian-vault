---
status: active
title: "Backup Rules"
summary: "Backup Rules - 備份、同步與還原規範"
description: "備份、同步與還原規範"
type: concept
tags: [backup, sync, flow, deploy]
created: 2026-06-21
updated: 2026-06-21
---

**Backup Rules**

備份、同步、還原前必須遵守。

**備份策略**

| 類型 | 工具 | 頻率 |
|:--|:--|:--|
| Git Backup | git push | 每次變更 |
| Database Backup | sqlite3 .dump | 每日 |
| Snapshot | rsync | 每日 |
| Full Backup | tar + gzip | 每週 |

**Git 備份原則**

- VPS 本機是主資料，GitHub 是備份區
- push 失敗時：pull rebase 若衝突則 abort → force push（以本機為準）
- 重要操作前：`git add -A && git commit -m "pre-op"`

**還原原則**

- 先備份再還原
- 還原後驗證資料完整性
- 重大還原需取得使用者核准

**安全規範**

- 備份檔案禁止存放在 raw/
- 敏感檔案（.env、API Key）禁止備份到 Git

**相關連結**

- [[schema]]：核心憲法
- [[policy]]：規則路由器
