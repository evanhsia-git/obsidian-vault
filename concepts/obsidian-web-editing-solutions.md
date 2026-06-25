---
status: active
title: "在網路上編輯 Obsidian 筆記的方案"
summary: "在網路上編輯 Obsidian 筆記的方案：1. CollabMD (本地檔案即時協作)"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [obsidian, integration, deploy]
---

# 在網路上編輯 Obsidian 筆記的方案

若需實現網路上即時編輯 Obsidian 筆記（而不僅僅是檢視），可參考以下三種成熟方案：

## 1. CollabMD (本地檔案即時協作)
這是目前最符合「直接編輯」需求的工具。
- **機制**：在 VPS 上運行，掛載 Obsidian Vault。提供與 Obsidian 極其相似的 Web 編輯器。
- **協作體驗**：多人同步編輯、游標同步、即時 Chat。
- **數據隱私**：所有檔案均存在於 VPS 本地硬碟，直接在檔案系統寫入。
- **適用場景**：團隊共編、審閱、遠端快速存取。

## 2. Obsidian LiveSync (跨裝置同步)
- **機制**：透過 Obsidian 插件與 CouchDB 資料庫，將筆記內容同步至不同端點（含瀏覽器）。
- **協作體驗**：提供即時同步機制，修改會即時推送到其他用戶端。
- **數據隱私**：資料庫完全由您自己掌握（建議在 VPS 上自行架設 CouchDB）。
- **適用場景**：個人多裝置同步、追求專業級同步體驗的用戶。

## 3. Hedgedoc (原 CodiMD)
- **機制**：強大的 Web Markdown 編輯器，支援 Mermaid、MathJax 等功能。
- **協作體驗**：與 Google Docs 類似，提供完善的權限控制（唯讀/編輯）。
- **注意事項**：非原生 Obsidian 格式（缺乏 原生關聯），通常需搭配同步機制使用。

---

## 綜合評估建議

| 工具 | 核心目標 | 同步方式 | 適合場景 | 難度 |
| :--- | :--- | :--- | :--- | :--- |
| **CollabMD** | 協作 & 檔案編輯 | 即時讀寫本地檔案 | 團隊共編、審閱、遠端存取 | 中 |
| **LiveSync** | 跨裝置同步 | 透過資料庫同步 | 個人多設備同步、專業用戶 | 高 |
| **Hedgedoc** | 文檔撰寫與協作 | 獨立編輯器 | 快速 Markdown 協作 | 低 |

## 實作建議 (針對本系統)
鑑於目前已擁有 VPS 環境，建議採用 **CollabMD** 作為首選。
- **優點**：不需同步資料庫，直接修改本地 Vault，完全對應現有 GitHub 備份機制，且 Agent 隨時可讀取更新後的內容。
- **後續實作**：可透過 `docker-compose` 部署 CollabMD，並設定密碼保護或零信任網路 (Zero Trust) 存取。

---
- [[openrouter-free-models]]
相關頁面：[[concepts/private-website-access|私人網站訪問]]