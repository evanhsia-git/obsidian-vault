---
title: Obsidian WebDAV Sync 使用指南
description: Obsidian WebDAV Sync 插件設定與同步機制摘要
summary: WebDAV 雙向同步插件，支援加密、衝突處理、增量同步
type: resource
status: active
tags: [obsidian, webdav, sync, plugin]
created: 2026-06-21
updated: 2026-06-21
---

# Obsidian WebDAV Sync

## 插件簡介

Obsidian WebDAV Sync 是雙向同步插件，將 Vault 與 WebDAV 伺服器同步。解決 vendor lock-in 問題，支援多裝置同步。

**License**：AGPL-3.0

---

## 核心功能

- **雙向同步**：本地與遠端保持一致
- **自動同步 + 衝突處理**：自動偵測變更，提供衝突解決策略
- **客戶端加密**：多層加密，比 Rclone Crypt 更安全
- **高效能**：Fast Mode、IndexedDB、大檔案分塊下載
- **可擴展**：支援大型 Vault 與頻繁同步

---

## 同步邏輯（三狀態判斷）

插件維護「上次同步記錄」，比對三種狀態決定動作：

- **Local + Remote + Record 都存在**：無變更則 Skip，有變更則衝突處理
- **只有 Local 變更**：Push 到遠端
- **只有 Remote 變更**：Pull 到本地
- **Local 不存在**：刪除遠端或拉取新檔
- **Remote 不存在**：推送本地或刪除本地
- **Record 不存在**：建立記錄

---

## 加密機制

使用 **AES-GCM-SIV-256**，加密因子包含：

- 使用者密碼、Master Key、Master Salt
- 帳號名稱、伺服器 URL、遠端目錄
- 檔案大小、檔案路徑
- 16 Byte 隨機 Salt、File Key Salt、Chunk Count

檔案路徑和大小也納入加密金鑰，可驗證完整性並偵測竄改。

---

## 效能優化

- **Fast Mode**：重用遠端狀態記錄，避免完整 WebDAV 遍歷（預設開啟）
- **IndexedDB**：高效儲存與查詢檔案記錄
- **分塊下載**：大檔案切割並行下載，低記憶體消耗，支援續傳

---

## 設定步驟

1. Obsidian → 社群插件 → 搜尋 **WebDAV Sync**（by Hēsperus）安裝
2. 設定 WebDAV 連線：
   - **URL**：`http://172.104.105.71/webdav`
   - **帳號**：`admin`
   - **密碼**：`hnQr+kLgxppG5ocuL5VG8lxuUAguE9RZ`
   - **遠端目錄**：`/`
3. 首次同步前**務必備份**

---

## 本機 WebDAV 伺服器資訊

- **Nginx** 架設，使用 `ngx_http_dav_module`
- **root**：`/root/Documents/Obsidian Vault`（alias 模式）
- **認證**：HTTP Basic Auth
- **密碼檔**：`/etc/nginx/.webdavpasswd`
- **CORS**：已啟用，支援瀏覽器客戶端

---
## 相關節點
- [[schema]]