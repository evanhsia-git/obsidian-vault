---
title: "Cron Job 列表"
description: "系統所有排程任務（Cron Job）的狀態、類型、執行時間與備註"
summary: "10 個排程任務：股票更新、新聞、Token 報告、系統維護、市值更新"
type: index
status: active
tags: [hermes, agent, auto]
created: 2026-06-27
updated: 2026-07-10
---

# Cron Job 列表（2026-07-10 更新）

| Job ID | 名稱 | 下次執行 | 類型 | 技能/腳本 | 狀態 | 備註 |
|--------|------|----------|------|-----------|------|----------|
| ed5d30a18e08 | Hermes Update Check | 2026-07-10 20:00 | Agent | - | 成功 | 每 3 天 20:00 檢查版本更新 |
| 545d8fb6a9e8 | 每日系統維護 | 2026-07-11 05:00 | Script (Shell) | user-backup.sh | 成功 | Lint+備份+Quartz 部署，每日 05:00 |
| 85b256249300 | 每日 Token 使用量報告 | 2026-07-11 08:00 | Script (Python) | token_usage_report.py | 成功 | Token 用量報告，每日 08:00 |
| e2532c7ccbda | 每日AI及科技新聞 | 2026-07-11 09:00 | Script (Python) | daily-news-tech.py | 成功 | AI/科技新聞，09:00、17:00 |
| ca7b49e89df2 | 每日股市指標 | 2026-07-11 08:30 | Script (Python) | daily-news-stock.py | 成功 | 亞洲指數、美國核心指標、CNN、匯率，每日 08:30 |
| 15ff71379857 | twstock-market-cap-daily | 2026-07-13 15:30 | Script (Shell) | market_cap_wrapper.sh | 成功 | 台股市值更新+驗證（no_agent，15:30 週一至週五）|
| cf916e0efd42 | 每日選股推薦 (Daily Stock Picker) | 2026-07-13 14:15 | Script (Shell) | daily_stock_pick_wrapper.sh | 成功 | no_agent → quant-trading/scripts/daily_stock_pick.py，14:15 週一至週五 |
| 4fd32dcca55c | twstock-daily-update-split | 2026-07-13 14:00 | Script (Shell) | batch1_wrapper.sh | 新建待跑 | no_agent → update_all.py --mode batch1，14:00 週一至週五 |
| 7d448e9b81a2 | twstock-daily-update-split-batch2 | 2026-07-13 14:30 | Script (Shell) | batch2_wrapper.sh | 新建待跑 | no_agent → update_all.py --mode batch2，14:30 週一至週五 |
| 9c3715467eee | twstock-daily-update-split-batch3 | 2026-07-13 15:00 | Script (Shell) | batch3_wrapper.sh | 新建待跑 | no_agent → update_all.py --mode batch3，15:00 週一至週五 |

---

## 類型統計

- 總 job：10
- no_agent 腳本型：9（ed5d30a18e08 除外，為 Agent 型但 model:null 跟隨全域，不會 drift）
- Agent 型：1（Hermes Update Check）
- 狀態異常：0

## 2026-07-10 變更記錄

- `ed5d30a18e08`：頻率由 `every 7200m`（每 2 小時）改為 `0 9 */3 * *`（每 3 天 09:00）
- `15ff71379857`（twstock-market-cap-daily）：由 Agent/Skill 型改為 `no_agent` + `market_cap_wrapper.sh`（完全腳本化，對服務商切換免疫）
- 3 個 batch job（4fd32dcca55c / 7d448e9b81a2 / 9c3715467eee）：本批為 2026-07-10 新建的 no_agent 腳本型（取代舊 Agent 型 e923159f4e35 / 8be985ea13fb / 62e2c14bfa17，已移除）

---

相關連結：[[index|主索引]] | [[system/system-index|System Index]] | [[skills/skills-index|Skills List]] | [[skills/troubleshooting|Skills Troubleshooting]] | [[hermes-agent|Hermes Agent Skill]]
