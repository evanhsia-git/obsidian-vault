---
title: "Hermes Agent 災難恢復與備份系統"
description: "多層備份 + VPS + 記憶架構 + 錯誤處理"
summary: "Hermes 災難恢復策略 + 備份系統 + 三層記憶模型"
type: concept
status: active
tags: [hermes, backup, vps]
created: 2026-06-23
updated: 2026-06-27
---

# Hermes Agent 災難恢復與備份系統

## 更新驗證步驟

```bash
git status --short           # 1. 代碼庫狀態
hermes doctor                # 2. 系統健康
hermes --version             # 3. 版本確認
hermes gateway status        # 4. 網關狀態
npm audit fix                # 5. 安全修復
```

## 安裝方式備份支援

| 安裝方式 | 備份支援 | 回滾方式 |
|:---|:---|:---|
| Git | 完整備份 | `git checkout <commit>` |
| pip | 版本檢查 | `pip install --upgrade` |
| Nix | flake + profile | `nix profile rollback` |

## 災難恢復策略

| 策略 | 觸發 | 機制 |
|:---|:---|:---|
| 自動備份 | 更新前 | 狀態快照 → `~/.hermes/backups/` |
| 快速回滾 | 失敗時 | Git 回滾 + 配置恢復（分鐘級） |
| 日誌分析 | 故障後 | `~/.hermes/logs/` 問題診斷 |
| 版本對比 | 定期 | GitHub releases + `hermes update --check` |

## 備份系統核心功能

- **Full Pre-Update Backup**：`hermes update --backup` 或 config `pre_update_backup: true`
- **自動化流程**：Pairing-data snapshot → Git pull → 語法驗證 → 依賴安裝 → Config migration → Gateway restart
- **安全性**：解析失敗自動回滾、輕量快照、日誌記錄
- **連續性**：忽略 SIGHUP、進度鏡像到日誌

## 多層備份

1. **Pairing-data snapshot** — 輕量預更新狀態
2. **Git 歷史** — 完整版本追蹤
3. **Config migration** — 配置變更記錄
4. **Gateway 狀態** — 服務連續性

## 三層記憶模型

| 層級 | 載體 | 職責 |
|:---|:---|:---|
| 事務記憶 | SQLite (state.db) | 對話歷史、任務狀態 |
| 事實記憶 | Memory + SOUL.md | 偏好、身份、核心定義 |
| 知識記憶 | Obsidian Wiki | 研究成果、方法論 |

## VPS 環境

- 主機：Linode 2GB (Tokyo 2) / Cloudflare Tunnel
- 備份策略：自動化 + 手動驗證

## 最佳實踐

- 重大更新前完整備份
- 定期檢查日誌異常
- 更新後完整性驗證
- 備份時間/版本記錄到 Wiki
