---
title: "Cron Job 列表"
description: "系統所有排程任務（Cron Job）的狀態、類型、執行時間與備註"
summary: "8 個排程任務：股票更新、新聞、Token 報告、系統維護"
type: index
status: active
tags: [hermes, agent, auto]
created: 2026-06-27
updated: 2026-06-27
---

# Cron Job 列表（2026-06-27 更新）

| Job ID | 名稱 | 下次執行 | 類型 | 技能/腳本 | 狀態 | 備註 |
|--------|------|----------|------|-----------|------|----------|
| ed5d30a18e08 | Hermes Update Check | 2026-06-28 21:46 | Agent | - | 成功 | 每 2 小時檢查版本更新 |
| 545d8fb6a9e8 | 每日系統維護 | 2026-06-27 05:00 | Script (Shell) | user-backup.sh | 成功 | Lint+備份+Quartz 部署 |
| e923159f4e35 | twstock-daily-update-split | 2026-06-29 14:00 | Skill | twse-stock-data | 成功 | Batch 1 — 收盤價 + PE/PB/殖利率 |
| 85b256249300 | 每日 Token 使用量報告 | 2026-06-27 08:00 | Script (Python) | token_usage_report.py | 成功 | Token 用量報告 |
| 8be985ea13fb | twstock-daily-update-split-batch2 | 2026-06-29 14:30 | Skill | twse-stock-data | 成功 | Batch 2 — 基本面更新至 stock_overview |
| 62e2c14bfa17 | twstock-daily-update-split-batch3 | 2026-06-29 15:00 | Skill | twse-stock-data | 成功 | Batch 3 — 收盤價+產業別+完整性檢查 |
| e2532c7ccbda | 每日AI及科技新聞 | 2026-06-27 09:00 | Script (Python) | daily-news-tech.py | 成功 | 09:00、17:00 |
| ca7b49e89df2 | 每日股市指標 | 2026-06-27 08:30 | Script (Python) | daily-news-stock.py | 成功 | 亞洲指數、美國核心指標、CNN、匯率 |

---

相關連結：[[index|主索引]] | [[system/system-index|System Index]] | [[skills/skills-index|Skills List]] | [[skills/troubleshooting|Skills Troubleshooting]] | [[hermes-agent|Hermes Agent Skill]]