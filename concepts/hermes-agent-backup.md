---
title: Hermes-Agent 備份系統
description: Hermes-Agent 備份系統 — 概念說明頁面
summary: "Hermes-Agent 備份系統：核心功能 + 特性 + 錯誤處理"
type: concept
status: active
priority: P2
tags: ["hermes", "maintenance", "concept"]
aliases: []
created: 2026-06-08
updated: 2026-06-23
publish: true
draft: false
---

# Hermes-Agent 備份系統

> 災難恢復策略、VPS 環境整合、最佳實踐詳見 [[hermes-disaster-recovery|Hermes Agent 災難恢復策略]]。

---

## 備份系統核心功能

### Full Pre-Update Backup 完整預更新備份

#### 手動觸發
```bash
hermes update --backup
```

#### 設定為預設行為
```yaml
# ~/.hermes/config.yaml
updates:
  pre_update_backup: true
```

### 自動化備份流程

1. **Pairing-data snapshot** - 輕量級預更新狀態快照
2. **Git pull** - 拉取最新代碼和更新子模組
3. **Post-pull syntax validation + auto-rollback** - 如果關鍵檔案解析失敗，自動回滾
4. **Dependency install** - 執行 `uv pip install -e ".[all]"`
5. **Config migration** - 檢測新配置選項並提示設定
6. **Gateway auto-restart** - 刷新正在運行的網關

---

## 備份系統特性

### 安全性保障
- **自動回滾**：關鍵檔案解析失敗時自動回滾
- **輕量級快照**：不影響系統性能的狀態記錄
- **日誌記錄**：更新過程日誌保存到 `~/.hermes/logs/update.log`

### 連續性保障
- **忽略 SIGHUP**：終端機斷開不會中斷更新
- **Ctrl-C 和系統關機仍受尊重**：用戶仍可控制
- **進度鏡像**：輸出同時記錄到日誌檔案

### 版本控制
- **Git 管理**：完整的版本歷史追蹤
- **分支支援**：支持非預設分支更新
- **版本檢查**：`hermes update --check` 檢查更新

---

## 多層備份策略

### 1. Pairing-data snapshot
- 類型：輕量級狀態快照
- 目的：更新前的狀態記錄
- 特點：不影響系統性能

### 2. Git 歷史
- 類型：完整代碼版本控制
- 目的：代碼變更歷史追蹤
- 特點：完整的版本歷史

### 3. Config migration
- 類型：配置變更記錄
- 目的：配置選項變更追蹤
- 特點：互動式配置遷移

### 4. Gateway 狀態
- 類型：服務連續性保障
- 目的：確保服務不中斷
- 特點：自動重啟機制

---

## 錯誤處理機制

### 語法驗證
- 自動檢查：更新後自動檢查語法
- 失敗處理：解析失敗時觸發回滾
- 日誌記錄：詳細的錯誤信息

### 自動回滾
- 觸發條件：關鍵檔案解析失敗
- 恢復機制：自動恢復到之前狀態
- 時間點：在依賴安裝前進行

### 日誌記錄
- 位置：`~/.hermes/logs/update.log`
- 內容：完整的更新過程記錄
- 用途：問題診斷和進度追蹤

---

## 相關連結
- [[hermes-disaster-recovery|Hermes Agent 災難恢復策略]] — 災難恢復 + VPS 環境 + 最佳實踐
- [[hermes-memory-system|記憶與知識系統架構]]
- [[hermes-hierarchy-architecture|Hermes Agent 記憶架構與階層圖]]
- [[hermes-configuration|Hermes Agent 配置說明]]
- [[skill-usage-protocol|Skill 使用規範]]

---

## 相關節點
- [[concepts/concepts-index|概念筆記索引]]
