---
title: "VPS Obsidian 與部落格部署方案"
description: "VPS Obsidian 與部落格部署方案 — 概念說明頁面"
summary: "VPS Obsidian 與部落格部署方案"
type: concept
status: active
priority: P2
tags: [deploy, obsidian]
aliases: []
created: 2026-06-11
updated: 2026-06-11
date: 2026-06-11
publish: true
draft: false
related:
source:
due:
review:
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
