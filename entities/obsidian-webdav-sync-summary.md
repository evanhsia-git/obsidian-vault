---
title: "Obsidian WebDAV Sync 插件指南"
description: "Obsidian WebDAV Sync 插件指南 — 實體資料頁面"
summary: "Obsidian WebDAV Sync 插件指南"

type: entity
status: active

tags: []

created: 2026-06-16
updated: 2026-06-16



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