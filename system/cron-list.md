---
status: active
title: "Cron Job List"
summary: "Hermes Agent 所有 cron 排程任務的技能與腳本對照清單"
description: "Hermes Agent cron job 完整列表：名稱、Job ID、排程、對應技能 (skill)、實際腳本路徑與執行模式"
type: resource
tags: [automation, workflow, hermes]
created: 2026-07-11
updated: 2026-07-11
---

# Cron Job List（排程任務對照表）

> 最後更新：2026-07-11。所有 `no_agent` job 的 script 位於 `/root/.hermes/scripts/`，由 cron 直接執行（對模型切換免疫）。
> Job ID 可透過 `cronjob list` 取得，修改/暫停用 `cronjob update/pause/remove`（需先 list 取得 job_id）。

## 對照表

| # | Job 名稱 | Job ID | 排程 | 技能 (Skill) | 腳本路徑 | 模式 |
|---|----------|--------|------|--------------|----------|------|
| 1 | 每日系統維護 (Lint+備份+Quartz) | `545d8fb6a9e8` | 每日 05:00 | `user-maintenance` / `obsidian-lint` / `user-backup`（shell 調用） | `/root/.hermes/scripts/user-backup.sh` | no_agent |
| 2 | 每日 Token 使用量報告 | `85b256249300` | 每日 08:00 | `token-usage` | `/root/.hermes/scripts/token_usage_report.py` | no_agent |
| 3 | 每日股市指標 | `ca7b49e89df2` | 每日 08:30 | `daily-news-stock-market` | `/root/.hermes/scripts/daily-news-stock.py` | no_agent |
| 4 | 每日 AI 及科技新聞 | `e2532c7ccbda` | 每日 09:00, 17:00 | `daily-news-technology` | `/root/.hermes/scripts/daily-news-tech.py` | no_agent |
| 5 | twstock 每日更新 split-batch1 | `4fd32dcca55c` | 週一~五 14:00 | `quant-trading` | `/root/.hermes/scripts/batch1_wrapper.sh` → `quant-trading/scripts/update_all.py --mode batch1` | no_agent |
| 6 | 每日選股推薦 (Daily Stock Picker) | `cf916e0efd42` | 週一~五 14:15 | `quant-trading` | `/root/.hermes/scripts/daily_stock_pick_wrapper.sh` → `quant-trading/scripts/daily_stock_pick.py` | no_agent |
| 7 | twstock 每日更新 split-batch2 | `7d448e9b81a2` | 週一~五 14:30 | `quant-trading` | `/root/.hermes/scripts/batch2_wrapper.sh` → `quant-trading/scripts/update_all.py --mode batch2` | no_agent |
| 8 | twstock 每日更新 split-batch3 | `9c3715467eee` | 週一~五 15:00 | `quant-trading` | `/root/.hermes/scripts/batch3_wrapper.sh` → `quant-trading/scripts/update_all.py --mode batch3` | no_agent |
| 9 | twstock 每日市值更新 | `15ff71379857` | 週一~五 15:30 | `quant-trading` | `/root/.hermes/scripts/market_cap_wrapper.sh` → `quant-trading/scripts/fetch_market_cap.py` | no_agent |
| 10 | Hermes Update Check | `ed5d30a18e08` | 每 3 日 20:00 | `hermes-agent`（Agent 模式） | 無（Agent 直接執行 `hermes --version`） | **Agent** |

## 關鍵說明

- **腳本位置**：所有 `no_agent` job 的 script 都放在 `/root/.hermes/scripts/`（頂層），由 cron 直接執行。
- **batch1/2/3 wrappers**：薄包裝層，實際邏輯在 `quant-trading/scripts/update_all.py`（canonical 腳本），透過 `--mode` 分流。這三個 job 即每日台股資料更新拆分鏈，**會自動補抓資料缺口**（如 TWSE 限流導致的 timeout 日）。
- **模型免疫**：9/10 個 job 為純腳本型（`no_agent: true`），不受 openrouter/gemini/nvidia 服務商切換影響；僅 #10 為 Agent 模式（需 LLM）。
- **#1 系統維護**：雖標 `no_agent`，但 `user-backup.sh` 內部調用 obsidian-lint / backup / quartz 等 skill 腳本，間接關聯多個 skill。

## 維護注意

- 新增 cron job 時，`skill` 欄位必須指向 **canonical 技能**（如 `quant-trading`），避免指向舊副本腳本（見技能合併原則）。
- 修改 Job ID 後需同步更新相關依賴（如 `context_from` 鏈）。
- 所有 job 的 `last_status` 應為 `ok` 或 `null`（待跑）；若出現 `fail` 需排查（參考 `user-maintenance` 技能）。
