---
title: "Obsidian 雲端托管與發布平台比較"
description: "Obsidian 雲端托管與發布平台比較 — 比較分析頁面"
summary: "Obsidian 雲端托管與發布平台比較"
type: concept
status: active
tags: [workflow]
created: 2026-06-12
updated: 2026-06-12
---

# Obsidian 雲端托管與發布平台比較

本表涵蓋 PKM、靜態發布、協作編輯、部落格 CMS 共 11 個平台，針對 Obsidian 使用者的需求進行深度評估。

## 🔑 評估基準說明

- ✅ **完整支援**
- ⚠️ **部分支援／需設定**
- ❌ **不支援**
- 💰 **付費限定**

## 📊 完整比較表

| 平台 | 類型 | 授權 | 免費方案限制 | 付費起價 | 全文搜尋 | Wikilink | Backlinks | Dataview | Obsidian 語法 | 線上編輯 | 資料匯出 | GitHub 整合 | 佈景主題 | Hermes Agent 適配 |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **── PKM ／ 知識圖譜型 ──** | | | | | | | | | | | | | | |
| [Quartz v5](https://quartz.jzhao.xyz) | 靜態發布 | MIT | 完全免費 (需 Node.js + Git 部署) | N/A | ✅ | ✅ | ✅ | ⚠️ | ✅ | ❌ | ✅ | ✅ | ✅ | 最高 |
| [Logseq](https://logseq.com) | 大綱 PKM | AGPL-3.0 | 核心功能免費 (Sync 需贊助) | ~$5/月 | ✅ | ✅ | ✅ | ⚠️ | ⚠️ | ✅ | ✅ | ✅ | ✅ | 高 |
| [AFFiNE](https://affine.pro) | 全功能工作區 | MIT+EE | 10 GB 雲端、3 成員、7 天歷史 | Pro $6.75/月 | ✅ | ⚠️ | ✅ | ❌ | ⚠️ | ✅ | ✅ | ⚠️ | ✅ | 中 |
| [SiYuan Note](https://b3log.org/siyuan) | 本地優先 PKM | AGPL-3.0 | 幾乎免費 (雲端 sync 需付費) | 未公開 | ✅ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ | ✅ | ⚠️ | ✅ | 中 |
| [TriliumNext](https://triliumnotes.org) | 層級知識庫 | AGPL-3.0 | 完全免費 (需自架) | N/A | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ | ✅ | ✅ | ❌ | ✅ | 中低 |
| [Obsidian + Git](https://obsidian.md) | 原生 Obsidian | 非開源 | 桌面版免費 (Sync $5/月; Publish $10/月) | $5/月 | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | 最高 |
| **── 協作 Markdown ／ 文件型 ──** | | | | | | | | | | | | | | |
| [HackMD](https://hackmd.io) | 協作 Markdown | 部分開源 | 公開筆記無限 (私有受限) | Prime $4-6/月 | ✅ | ❌ | ❌ | ❌ | ⚠️ | ✅ | ✅ | ✅ | ⚠️ | 中 |
| [GitBook](https://gitbook.com) | 技術文件平台 | 非開源 | 開源專案免費 (協作/私有需付費) | $65/站/月 | ✅ | ⚠️ | ⚠️ | ❌ | ⚠️ | ✅ | ✅ | ✅ | ⚠️ | 中 |
| [Foam](https://foamnotes.com) | VS Code 插件 | MIT | 完全免費 (需 VS Code + Git) | N/A | ✅ | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ✅ | ✅ | ⚠️ | 高 |
| **── 部落格 ／ CMS 型 ──** | | | | | | | | | | | | | | |
| [Ghost](https://ghost.org) | 部落格 CMS | MIT | 自架免費 (Ghost Pro 起 $36/月) | $36/月起 | ✅ | ❌ | ❌ | ❌ | ⚠️ | ✅ | ✅ | ⚠️ | ✅ | 低中 |
| [Wiki.js](https://js.wiki) | Wiki 平台 | AGPL-3.0 | 完全免費 (需自架) | N/A | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | ✅ | ✅ | ✅ | ✅ | 中 |

---
## 相關節點
- [[keystonejs]]
- [[private-website-access]]

## Website Deployment
---
status: active
title: "如何將 Obsidian 發佈為私人入口網站"
summary: "如何將 Obsidian 發佈為私人入口網站：推薦方案：Quartz + Cloudflare Zero Trust"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [quartz, deploy]
---

# 如何將 Obsidian 發佈為私人入口網站

本頁面記錄將 Obsidian Vault 轉變為知識庫網站的方法，特別是針對「私人檢視」需求的實作方案。

## 推薦方案：Quartz + Cloudflare Zero Trust

考慮到安全性與便利性，推薦使用 Quartz 進行靜態建置，並結合 Cloudflare Zero Trust 進行私人防護。

### 1. Quartz (靜態網站生成)
Quartz 是目前 Obsidian 社群中最成熟的數位花園工具，完美支援：
- ``、Backlinks
- Mermaid 圖表、Callouts
- 快速搜尋、SEO 優化

### 2. 私人檢視解決方案
由於 Quartz 預設為公開，若需達成私人檢視，建議採用以下方式：

#### 方案 A：Cloudflare Zero Trust (最強烈推薦)
- **原理**：將 Quartz 部署於 Cloudflare Pages，並在 Cloudflare 儀表板設定 Zero Trust Access。
- **優點**：不需維護伺服器，由 Cloudflare 提供免費且強大的身分驗證（Email OTP 或 Google 登入），非授權者無法存取。

#### 方案 B：VPS Nginx + Basic Auth
- **原理**：在 VPS 上使用 `npx quartz build` 生成靜態檔，並設定 Nginx 進行帳號密碼保護。
- **優點**：完全自行掌控資料，不依賴第三方雲端服務。

---

## 實作路徑建議
1. **本地測試**：在 VPS 上進行 Quartz 本地建置，確認介面符合預期。
2. **部署選型**：
   - 若傾向「無維護成本」：選擇方案 A。
   - 若傾向「極致隱私」：選擇方案 B。
3. **自動化**：透過 GitHub Actions 或自動化腳本，確保 Vault 更新後網站隨之同步。

- [[openrouter-free-models]]
## 相關工具連結
- [Quartz Official Site](https://quartz.jzhao.xyz/)
- [GitHub Pages](https://pages.github.com/)
- [Cloudflare Zero Trust](https://www.cloudflare.com/zero-trust/)

---
相關頁面：[[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]

## VPS Blog Deployment
---
title: "VPS Obsidian 與部落格部署方案"
description: "VPS Obsidian 與部落格部署方案 — 概念說明頁面"
summary: "VPS Obsidian 與部落格部署方案"
type: concept
status: active
tags: [deploy]
created: 2026-06-11
updated: 2026-06-11
---

# 🏗️ VPS Obsidian 與部落格部署方案

這份文件規劃了如何將 VPS 上的 Obsidian Vault 轉化為一個具備「網頁編輯」與「自動化發佈」能力的個人知識站點。

## 🎯 設計目標

1.  **雲端編輯**：能夠透過瀏覽器隨時隨地編輯筆記。
2.  **雙向感知**：當筆記變動時，Agent 能主動感知並執行後續動作（Lint/Sync/Log）。
3.  **自動發佈**：透過 GitHub Actions 將內容同步至靜態網站（Quartz）。
4.  **閉環流程**：實現 `編輯 $\rightarrow$ 感知 $\rightarrow$ 整理 $\rightarrow$ 發佈` 的完整循環。

## 🛠️ 技術棧 (Tech Stack)

| 層級 | 組件 | 說明 |
| :--- | :--- | :--- |
| **編輯端** | `code-server` | 瀏覽器版的 VS Code，直接對應 VPS 上的檔案系統。 |
| **安全層** | `Cloudflare Tunnel` | 提供安全、免開 Port 的外部存取路徑。 |
| **監聽端** | `inotify-tools` / `watchdog` | 監控檔案系統變動，觸發 Agent 任務。 |
| **核心層** | `Hermes Agent` | 執行內容檢查 (Lint)、記錄 Log、自動化同步。 |
| **同步端** | `Git` + `GitHub Actions` | 將變更推送至 GitHub 並自動觸發 Quartz 部署。 |
| **展示層** | `Quartz v5` | 基於 Markdown 的靜態網站生成器。 |

## 🔄 核心工作流 (Workflow)

### 1. 編輯階段
使用者透過 `code-server` 進入 Web 介面，直接編輯 `/root/Documents/Obsidian Vault` 中的 Markdown 檔案。

### 2. 感知階段 (The Watcher)
當檔案儲存後，`Watcher` 腳本偵測到 `MODIFY` 或 `CREATE` 事件：
1.  **觸發 Agent**：腳本呼叫 Agent 處理該檔案。
2.  **內容檢查**：Agent 檢查 YAML Frontmatter 與 Markdown 語法。
3.  **知識沉澱**：Agent 將變動資訊寫入 `log.md`。

### 3. 發佈階段
Agent 完成檢查後，執行同步腳本：
1.  `git add .`
2.  `git commit -m "Auto-sync: [檔案名]"`
3.  `git push origin main`
4.  **GitHub Actions** 自動接手進行 `Quartz Build` 與 `GitHub Pages` 部署。

## 🚀 實作路徑 (Roadmap)

- [ ] **Phase 1: 基礎設施搭建**
    - [ ] 在 Linode 上安裝並配置 `code-server`。
    - [ ] 設定 Cloudflare Tunnel 確保 Web 介面安全。
- [ ] **Phase 2: 感知機制開發**
    - [ ] 編寫 Python `watchdog` 監聽腳本。
    - [ ] 建立 Agent 處理邏輯 (Linting & Logging)。
- [ ] **Phase 3: 流程整合**
    - [ ] 將 Watcher 與 Git 同步腳本串聯。
    - [ ] 驗證從「網頁編輯」到「網站更新」的全流程。

---
**相關連結：**
- [[obsidian-web-editing-solutions|在網路上編輯 Obsidian 筆記]]
- [[vps-obsidian-blog-deployment|部署方案概覽]]
- [[hermes-agent-strategy|Hermes Agent 執行策略]]

## Website Deployment


考慮到安全性與便利性，推薦使用 Quartz 進行靜態建置，並結合 Cloudflare Zero Trust 進行私人防護。

### 1. Quartz (靜態網站生成)
Quartz 是目前 Obsidian 社群中最成熟的數位花園工具，完美支援：
- ``、Backlinks
- Mermaid 圖表、Callouts
- 快速搜尋、SEO 優化

### 2. 私人檢視解決方案
由於 Quartz 預設為公開，若需達成私人檢視，建議採用以下方式：

#### 方案 A：Cloudflare Zero Trust (最強烈推薦)
- **原理**：將 Quartz 部署於 Cloudflare Pages，並在 Cloudflare 儀表板設定 Zero Trust Access。
- **優點**：不需維護伺服器，由 Cloudflare 提供免費且強大的身分驗證（Email OTP 或 Google 登入），非授權者無法存取。

#### 方案 B：VPS Nginx + Basic Auth
- **原理**：在 VPS 上使用 `npx quartz build` 生成靜態檔，並設定 Nginx 進行帳號密碼保護。
- **優點**：完全自行掌控資料，不依賴第三方雲端服務。

---

## 實作路徑建議
1. **本地測試**：在 VPS 上進行 Quartz 本地建置，確認介面符合預期。
2. **部署選型**：
   - 若傾向「無維護成本」：選擇方案 A。
   - 若傾向「極致隱私」：選擇方案 B。
3. **自動化**：透過 GitHub Actions 或自動化腳本，確保 Vault 更新後網站隨之同步。

- [[openrouter-free-models]]
## 相關工具連結
- [Quartz Official Site](https://quartz.jzhao.xyz/)
- [GitHub Pages](https://pages.github.com/)
- [Cloudflare Zero Trust](https://www.cloudflare.com/zero-trust/)

---
相關頁面：[[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]
## VPS Blog Deployment

## 🎯 設計目標

1.  **雲端編輯**：能夠透過瀏覽器隨時隨地編輯筆記。
2.  **雙向感知**：當筆記變動時，Agent 能主動感知並執行後續動作（Lint/Sync/Log）。
3.  **自動發佈**：透過 GitHub Actions 將內容同步至靜態網站（Quartz）。
4.  **閉環流程**：實現 `編輯 $\rightarrow$ 感知 $\rightarrow$ 整理 $\rightarrow$ 發佈` 的完整循環。

## 🛠️ 技術棧 (Tech Stack)

| 層級 | 組件 | 說明 |
| :--- | :--- | :--- |
| **編輯端** | `code-server` | 瀏覽器版的 VS Code，直接對應 VPS 上的檔案系統。 |
| **安全層** | `Cloudflare Tunnel` | 提供安全、免開 Port 的外部存取路徑。 |
| **監聽端** | `inotify-tools` / `watchdog` | 監控檔案系統變動，觸發 Agent 任務。 |
| **核心層** | `Hermes Agent` | 執行內容檢查 (Lint)、記錄 Log、自動化同步。 |
| **同步端** | `Git` + `GitHub Actions` | 將變更推送至 GitHub 並自動觸發 Quartz 部署。 |
| **展示層** | `Quartz v5` | 基於 Markdown 的靜態網站生成器。 |

## 🔄 核心工作流 (Workflow)

### 1. 編輯階段
使用者透過 `code-server` 進入 Web 介面，直接編輯 `/root/Documents/Obsidian Vault` 中的 Markdown 檔案。

### 2. 感知階段 (The Watcher)
當檔案儲存後，`Watcher` 腳本偵測到 `MODIFY` 或 `CREATE` 事件：
1.  **觸發 Agent**：腳本呼叫 Agent 處理該檔案。
2.  **內容檢查**：Agent 檢查 YAML Frontmatter 與 Markdown 語法。
3.  **知識沉澱**：Agent 將變動資訊寫入 `log.md`。

### 3. 發佈階段
Agent 完成檢查後，執行同步腳本：
1.  `git add .`
2.  `git commit -m "Auto-sync: [檔案名]"`
3.  `git push origin main`
4.  **GitHub Actions** 自動接手進行 `Quartz Build` 與 `GitHub Pages` 部署。

## 🚀 實作路徑 (Roadmap)

- [ ] **Phase 1: 基礎設施搭建**
    - [ ] 在 Linode 上安裝並配置 `code-server`。
    - [ ] 設定 Cloudflare Tunnel 確保 Web 介面安全。
- [ ] **Phase 2: 感知機制開發**
    - [ ] 編寫 Python `watchdog` 監聽腳本。
    - [ ] 建立 Agent 處理邏輯 (Linting & Logging)。
- [ ] **Phase 3: 流程整合**
    - [ ] 將 Watcher 與 Git 同步腳本串聯。
    - [ ] 驗證從「網頁編輯」到「網站更新」的全流程。

---
**相關連結：**
- [[obsidian-web-editing-solutions|在網路上編輯 Obsidian 筆記]]
- [[vps-obsidian-blog-deployment|部署方案概覽]]
- [[hermes-agent-strategy|Hermes Agent 執行策略]]
