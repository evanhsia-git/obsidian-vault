---
title: "Obsidian WebDAV Sync 使用指南"
description: "Obsidian WebDAV Sync 插件設定與同步機制摘要"
summary: "WebDAV 雙向同步插件，支援加密、衝突處理、增量同步"
type: resource
status: active
tags: [linux, sync, integration]
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
## WebDAV Sync Summary

---
# 🛠️ Obsidian WebDAV Sync 插件指南

## 🚀 安裝流程 (Installation Steps)

安裝過程非常直覺，主要透過 Obsidian 內建的插件商店完成：

1.  **進入插件商店**：開啟 Obsidian $\rightarrow$ 進入 `Settings` (設定) $\rightarrow$ 點選 `Community Plugins` (社群插件)。
2.  **搜尋插件**：點擊 `Browse` (瀏覽)，在搜尋框輸入 **"WebDAV Sync"**。
3.  **辨識作者**：請確認作者為 **"Hēsperus"**，以確保下載的是正確的插件。
4.  **安裝與啟用**：點擊 `Install` (安裝) $\rightarrow$ 完成後點擊 `Enable` (啟用)。
5.  **進行設定**：啟用後，進入該插件的設定頁面，填入您的 WebDAV 伺服器資訊（URL、帳號、密碼/Token）並設定同步規則。

---

## 📦 必備工具與資源 (Required Tools & Infrastructure)

要讓這個插件成功運作，您手邊必須具備以下資源：

### 1. 核心軟體
* **Obsidian App**：作為筆記編輯與插件運行的主程式。

### 2. 儲存後端 (WebDAV Server)
這是插件存放資料的核心。您可以使用以下任一工具建立：
* **NAS 設備** (如 Synology, QNAP)：內建 WebDAV 服務。
* **雲端儲存服務** (如 Nextcloud, Koofr, 或其他支援 WebDAV 協定的雲端空間)。
* **自行架設的伺服器** (如使用 Apache 或 Nginx 搭建的 WebDAV 環境)。

### 3. 認證資訊 (Credentials)
* **WebDAV URL**：您的伺服器端點位址 (例如 `https://your-nas.com/webdav/`)。
* **身分驗證資料**：您的 WebDAV 帳號名稱與密碼，或是專用的 Access Token。

### 4. (進階建議) 安全管理工具
* **Obsidian Keychain**：建議利用 Obsidian 內建的 Keychain 功能來管理您的 WebDAV 密鑰 (Secret)，這比直接在設定欄位輸入明文密碼更安全。

---

## ⚙️ 設定關鍵提示

* **`.obsidian` 資料夾同步**：
    * **預設行為**：為避免設定衝突，插件預設會**排除** `.obsidian` 資料夾。
    * **特定檔案同步**：若需同步特定設定（如 `app.json`），需在 `Filter Rules` $\rightarrow$ `Include Rules` 中手動加入路徑。
    * **完整同步**：若需同步整個設定資料夾，需從 `Exclude Rules` 中移除對 `.obsidian` 的排除規則。
* **速率限制 (Rate Limiting)**：若遇到頻率限制錯誤，請調整插件設定中的速率控制策略 (Rate Control Strategies)。

---
## 相關節點
- [[obsidian-webdav-sync]]
- [[vps-config]]