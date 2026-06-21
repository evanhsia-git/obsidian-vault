---
title: Hermes-Agent 備份系統
description: Hermes-Agent 備份系統 — 概念說明頁面
summary: Hermes-Agent 備份系統
type: concept
status: active
priority: P2
tags: ["hermes", "maintenance", "concept"]
aliases: []
created: 2026-06-08
updated: 2026-06-08
date: 2026-06-08
publish: true
draft: false
related:
source:
due:
review:
---

# Hermes-Agent 備份系統

本頁面記錄 Hermes-Agent 的完整備份系統架構、災難恢復策略和版本控制機制。

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

## 多層備份策略

### 1. Pairing-data snapshot
- **類型**：輕量級狀態快照
- **目的**：更新前的狀態記錄
- **特點**：不影響系統性能

### 2. Git 歷史
- **類型**：完整代碼版本控制
- **目的**：代碼變更歷史追蹤
- **特點**：完整的版本歷史

### 3. Config migration
- **類型**：配置變更記錄
- **目的**：配置選項變更追蹤
- **特點**：互動式配置遷移

### 4. Gateway 狀態
- **類型**：服務連續性保障
- **目的**：確保服務不中斷
- **特點**：自動重啟機制

## 錯誤處理機制

### 語法驗證
- **自動檢查**：更新後自動檢查語法
- **失敗處理**：解析失敗時觸發回滾
- **日誌記錄**：詳細的錯誤信息

### 自動回滾
- **觸發條件**：關鍵檔案解析失敗
- **恢復機制**：自動恢復到之前狀態
- **時間點**：在依賴安裝前進行

### 日誌記錄
- **位置**：`~/.hermes/logs/update.log`
- **內容**：完整的更新過程記錄
- **用途**：問題診斷和進度追蹤

## 推薦的更新驗證步驟

### 1. 代碼庫狀態檢查
```bash
git status --short
```
- **目的**：檢查代碼庫是否意外變更
- **預期**：乾淨狀態或預期的變更

### 2. 系統健康檢查
```bash
hermes doctor
```
- **目的**：檢查配置、依賴和服務健康
- **覆蓋**：配置完整性、依賴狀態、服務可用性

### 3. 版本確認
```bash
hermes --version
```
- **目的**：確認版本更新成功
- **對比**：與 GitHub releases 版本比對

### 4. 網關狀態檢查
```bash
hermes gateway status
```
- **目的**：檢查網關運行狀態
- **適用**：使用 gateway 的環境

### 5. 安全問題修復
```bash
npm audit fix
```
- **目的**：修復 npm 安全問題
- **適用**：使用 npm 套件的環境

## 不同安裝方式的備份支援

### Git 安裝
- **備份支援**：完整的備份和回滾支援
- **回滾方式**：`git checkout <commit-hash>`
- **優勢**：完整的版本歷史追蹤

### pip 安裝
- **備份支援**：版本檢查和更新支援
- **回滾方式**：`pip install --upgrade hermes-agent`
- **限制**：無法回滾到特定版本

### Nix 安裝
- **備份支援**：flake 和 profile 級別備份
- **更新方式**：`nix flake update hermes-agent`
- **回滾方式**：`nix profile rollback`

## 在您的環境中的應用

### VPS 環境整合
- **主機**：Linode 2GB (Tokyo 2)
- **IP**：172.104.105.71
- **特色**：透過 Cloudflare Tunnel 安全訪問
- **備份策略**：自動化備份 + 手動驗證

### Obsidian Vault 整合
- **路徑**：`/root/Documents/Obsidian Vault`
- **內容**：包含 Wiki 結構和技能文件
- **備份重點**：知識記憶層的完整性

### 災難恢復策略

#### 1. 自動備份
- **觸發**：更新前自動狀態快照
- **內容**：配置、狀態、日誌
- **位置**：`~/.hermes/backups/`

#### 2. 快速回滾
- **觸發**：失敗時自動恢復
- **機制**：Git 回滾 + 配置恢復
- **時間**：分鐘級別恢復

#### 3. 日誌分析
- **位置**：`~/.hermes/logs/`
- **內容**：詳細的問題診斷
- **用途**：故障排除和優化

#### 4. 版本對比
- **來源**：GitHub releases
- **工具**：`hermes version` 和 `hermes update --check`
- **目的**：確保使用最新穩定版本

## 最佳實踐建議

### 定期備份
- **頻率**：重大更新前執行完整備份
- **驗證**：更新後進行完整性檢查
- **記錄**：備份時間和版本記錄到 Wiki

### 監控與警報
- **日誌監控**：定期檢查更新日誌
- **異常檢測**：識別失敗模式
- **自動化**：設定定期健康檢查

### 文档維護
- **更新記錄**：記錄每次更新和變更
- **故障排除**：建立常見問題解決方案
- **團隊協作**：共享備份和恢復流程

## 相關連結
- [[hermes-memory-system|記憶與知識系統架構]] — 記憶系統整體架構
- [[hermes-hierarchy-architecture|Hermes Agent 記憶架構與階層圖]] — 權限與儲存分工
- [[hermes-configuration|Hermes Agent 配置說明]] — 配置文件結構
- [[skill-usage-protocol|Skill 使用規範]] — 技能管理最佳實踐


## 相關節點
- [[index]]
