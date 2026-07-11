---
title: "KeystoneJS"
description: "KeystoneJS — 概念說明頁面"
summary: "KeystoneJS"
type: concept
status: active
tags: []
created: 2026-06-13
updated: 2026-06-13
---

# KeystoneJS

KeystoneJS 是一個功能強大的 Headless CMS 與應用程式框架，專為 Node.js 設計，核心採用 GraphQL 與 React 構建。

## 核心特性

* **GraphQL API 與管理介面**：自動生成 API 並提供使用者友善的介面來管理內容與數據。
* **無樣板程式碼 (No Boilerplate)**：開發者可以專注於 Schema 定義，無需處理繁瑣的後端樣板程式碼。
* **現代技術棧**：利用 GraphQL 與 React 提供強大且具擴展性的開發體驗。

## 使用規範

* **NPM 套件命名空間**：Keystone 6 使用 `@keystone-6/*`。
* **專案初始化**：可使用 `create-keystone-app` 工具進行快速設定。
* **官方文件**：詳見 [KeystoneJS 官網](https://keystonejs.com/docs)。

## 版本資訊

* **Keystone 6**：目前的主流版本。
* **Keystone 5**：已進入維護模式 (Maintenance Mode)，原始碼位於 [keystonejs/keystone-5](https://github.com/keystonejs/keystone-5)。


- [[obsidian-cms]]
- private-website-access

## 相關連結

* **GitHub 倉庫**：[https://github.com/keystonejs/keystone](https://github.com/keystonejs/keystone)
* **社群討論**：[Slack](https://community.keystonejs.com/)
* **路線圖**：[Roadmap](https://keystonejs.com/updates/roadmap)

## Private Website Access

---
status: active
title: "私人網站訪問"
summary: "私人網站訪問：1. GitHub Pages 私人權限 (適用於企業/團隊帳號)"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [quartz, deploy]
---

# 私人網站訪問

針對 Quartz 建立的入口網站，若需要「私人檢視」而不是公開方式，可採取以下幾種實作方法：

## 1. GitHub Pages 私人權限 (適用於企業/團隊帳號)
- **適用條件**：您使用 GitHub Pro、Team 或 Enterprise 帳號。
- **設定方式**：
  1. 在 GitHub 儲存庫設定 → 魚叉圖示 → *Settings* → *Pages*。
  2. 將「Pages access」改為「Private」（僅限指定成員）。
  3. 將想要的使用者（例如團隊成員）加入「Collaborators」。
- **限制**：免費版個人帳號仍需公開，此方式僅適用於企業版。

## 2. VPS 部署 Quartz (完全私有)
- **部署流程**：
  1. 在 VPS 上執行 `npx quartz build`，產生 `public/` 資料夾。
  2. 安裝輕量網頁伺服器（如 Nginx 或 Caddy）。
  3. 將 `public/` 設定為伺服器根目錄。
  4. 透過以下方式實現私人存取：
     - **Nginx Basic Auth**：設定密碼保護。
     - **Cloudflare Zero Trust**：使用 Email OTP 或 Google 登入進行身份驗證。
- **優點**：完全自行掌控，無依賴公開平台。

## 3. Cloudflare Zero Trust (強烈推薦)
- **原理**：部署於 Cloudflare Pages，並啟用 Zero Trust Access。
- **設定方式**：
  1. 在 Cloudflare Dashboard → *Zero Trust* → *Access* → *Create Application*。
  2. 設定策略：僅允許特定 Email（如您的公司員工）存取。
  3. 使用 Google OIDC 或 Email OTP 驗證。
- **優點**：無需維護伺服器，搭配 Cloudflare 強大防護，且使用者體驗良好。

## 實作建議
1. **先測後部署**：建議在 VPS 上先完成 `npx quartz build`，確認生成的靜態檔案符合預期。
2. **自動化**：可透過 GitHub Actions 或腳本，自動化將 Vault 內容同步至 Quartz，再部署至選擇的平台。
3. **安全考量**：即使是私人網站，也建議啟用 HTTPS，並定期更新憑證。


- [[keystonejs]]
- [[obsidian-cms]]

## 相關工具說明
- [Quartz 官方文件](https://quartz.jzhao.xyz/)
- [Cloudflare Zero Trust 設定指南](https://developers.cloudflare.com/zero-trust/)
- [Nginx Basic Auth 設定](https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html)

--- 
== 

## ByteRover Summary

---
title: "ByteRover 記憶系統技術摘要"
description: "ByteRover 記憶系統技術摘要 — 概念說明頁面"
summary: "ByteRover 記憶系統技術摘要"
type: concept
status: active
tags: [agent, workflow]
created: 2026-06-05
updated: 2026-06-05
---

# ByteRover 記憶系統技術摘要

本頁面彙整 ByteRover 的官方技術文件與相關學術摘要，作為本系統未來記憶層級擴充的參考。

## 1. 核心概念
ByteRover 是一套「Agent-Native (代理原生)」的檔案型記憶架構，旨在解決 LLM 在多會話中資訊遺失、語義漂移 (Semantic Drift) 與協調性破碎的問題。

## 2. 關鍵技術架構
ByteRover 不同於傳統 Vector Database，其設計思維與我們的 Obsidian Wiki 類似，但具備更強的自動化維護能力：

- **Context Tree (知識樹)**：結構為 `Domain >> Topic >> Subtopic >> Entry`。以 Markdown 檔案為基礎，強調人類可讀性。
- **Adaptive Knowledge Lifecycle (AKL)**：
    - **重要性評分**：追蹤存取紀錄，引入權重與每日衰減 (Decay)。
    - **成熟度分級**：內容分級為 `draft` → `validated` → `core`。
- **5-Tier Progressive Retrieval (五級漸進檢索)**：
    - 透過快取 (Tier 0-2)、優化 LLM 搜尋 (Tier 3) 到完全代理迴圈 (Tier 4)，實現 sub-100ms 的檢索延遲。
- **Agent-Native 工具鏈**：提供 `curate`, `query`, `search` 作為 LLM 的一等公民工具。

## 3. 與 Hermes Wiki 的對照分析

| 比較項目 | 本系統 (Hermes 原生 Wiki) | ByteRover |
| :--- | :--- | :--- |
| **儲存格式** | Markdown (人工+Agent 維護) | Markdown (Agent 自主 Curate) |
| **檢索機制** | 導航式搜尋 (SCHEMA/index) | 5-Tier 漸進式檢索 (機器優先) |
| **維護方式** | 強制手動 + Agent Linting | 自動化 Curate 管道 |
| **可讀性** | 高 (人類友善) | 中 (專為 Agent Reasoning 設計) |

## 4. 對本系統的啟發
ByteRover 的 **AKL (自適應生命週期管理)** 與 **5-Tier 檢索技術** 極具參考價值。未來若我們的 Wiki 規模大幅擴張，可以考慮：
1. **引入權重機制**：在 `SCHEMA.md` 中增加知識的成熟度分類。
2. **優化檢索**：參考 Progressive Retrieval 的分層邏輯，將現有的全文搜尋優化為多級別觸發。

## 5. 結論
ByteRover 的技術證明了「以檔案為基礎的結構化知識」確實是目前 Agent 記憶的最佳實踐。我們目前的 Obsidian 系統在結構上與 ByteRover 高度相容，未來轉型至 ByteRover 協議的門檻極低。目前建議維持現行架構，待資料量觸發效能瓶頸時再考慮自動化 Curate 導入。

---
*來源：[ByteRover Official Docs](https://www.byterover.dev/), [ArXiv 2604.01599](https://arxiv.org/html/2604.01599v1)*


相關頁面：hermes-memory-system

相關頁面：hermes-hierarchy-architecture


## 相關節點
- [[index]]

## Private Website Access

- **適用條件**：您使用 GitHub Pro、Team 或 Enterprise 帳號。
- **設定方式**：
  1. 在 GitHub 儲存庫設定 → 魚叉圖示 → *Settings* → *Pages*。
  2. 將「Pages access」改為「Private」（僅限指定成員）。
  3. 將想要的使用者（例如團隊成員）加入「Collaborators」。
- **限制**：免費版個人帳號仍需公開，此方式僅適用於企業版。

## 2. VPS 部署 Quartz (完全私有)
- **部署流程**：
  1. 在 VPS 上執行 `npx quartz build`，產生 `public/` 資料夾。
  2. 安裝輕量網頁伺服器（如 Nginx 或 Caddy）。
  3. 將 `public/` 設定為伺服器根目錄。
  4. 透過以下方式實現私人存取：
     - **Nginx Basic Auth**：設定密碼保護。
     - **Cloudflare Zero Trust**：使用 Email OTP 或 Google 登入進行身份驗證。
- **優點**：完全自行掌控，無依賴公開平台。

## 3. Cloudflare Zero Trust (強烈推薦)
- **原理**：部署於 Cloudflare Pages，並啟用 Zero Trust Access。
- **設定方式**：
  1. 在 Cloudflare Dashboard → *Zero Trust* → *Access* → *Create Application*。
  2. 設定策略：僅允許特定 Email（如您的公司員工）存取。
  3. 使用 Google OIDC 或 Email OTP 驗證。
- **優點**：無需維護伺服器，搭配 Cloudflare 強大防護，且使用者體驗良好。

## 實作建議
1. **先測後部署**：建議在 VPS 上先完成 `npx quartz build`，確認生成的靜態檔案符合預期。
2. **自動化**：可透過 GitHub Actions 或腳本，自動化將 Vault 內容同步至 Quartz，再部署至選擇的平台。
3. **安全考量**：即使是私人網站，也建議啟用 HTTPS，並定期更新憑證。


- [[keystonejs]]
- [[obsidian-cms]]

## 相關工具說明
- [Quartz 官方文件](https://quartz.jzhao.xyz/)
- [Cloudflare Zero Trust 設定指南](https://developers.cloudflare.com/zero-trust/)
- [Nginx Basic Auth 設定](https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html)

---
== 
## ByteRover Summary

## 1. 核心概念
ByteRover 是一套「Agent-Native (代理原生)」的檔案型記憶架構，旨在解決 LLM 在多會話中資訊遺失、語義漂移 (Semantic Drift) 與協調性破碎的問題。

## 2. 關鍵技術架構
ByteRover 不同於傳統 Vector Database，其設計思維與我們的 Obsidian Wiki 類似，但具備更強的自動化維護能力：

- **Context Tree (知識樹)**：結構為 `Domain >> Topic >> Subtopic >> Entry`。以 Markdown 檔案為基礎，強調人類可讀性。
- **Adaptive Knowledge Lifecycle (AKL)**：
    - **重要性評分**：追蹤存取紀錄，引入權重與每日衰減 (Decay)。
    - **成熟度分級**：內容分級為 `draft` → `validated` → `core`。
- **5-Tier Progressive Retrieval (五級漸進檢索)**：
    - 透過快取 (Tier 0-2)、優化 LLM 搜尋 (Tier 3) 到完全代理迴圈 (Tier 4)，實現 sub-100ms 的檢索延遲。
- **Agent-Native 工具鏈**：提供 `curate`, `query`, `search` 作為 LLM 的一等公民工具。

## 3. 與 Hermes Wiki 的對照分析

| 比較項目 | 本系統 (Hermes 原生 Wiki) | ByteRover |
| :--- | :--- | :--- |
| **儲存格式** | Markdown (人工+Agent 維護) | Markdown (Agent 自主 Curate) |
| **檢索機制** | 導航式搜尋 (SCHEMA/index) | 5-Tier 漸進式檢索 (機器優先) |
| **維護方式** | 強制手動 + Agent Linting | 自動化 Curate 管道 |
| **可讀性** | 高 (人類友善) | 中 (專為 Agent Reasoning 設計) |

## 4. 對本系統的啟發
ByteRover 的 **AKL (自適應生命週期管理)** 與 **5-Tier 檢索技術** 極具參考價值。未來若我們的 Wiki 規模大幅擴張，可以考慮：
1. **引入權重機制**：在 `SCHEMA.md` 中增加知識的成熟度分類。
2. **優化檢索**：參考 Progressive Retrieval 的分層邏輯，將現有的全文搜尋優化為多級別觸發。

## 5. 結論
ByteRover 的技術證明了「以檔案為基礎的結構化知識」確實是目前 Agent 記憶的最佳實踐。我們目前的 Obsidian 系統在結構上與 ByteRover 高度相容，未來轉型至 ByteRover 協議的門檻極低。目前建議維持現行架構，待資料量觸發效能瓶頸時再考慮自動化 Curate 導入。

---
*來源：[ByteRover Official Docs](https://www.byterover.dev/), [ArXiv 2604.01599](https://arxiv.org/html/2604.01599v1)*


相關頁面：hermes-memory-system

相關頁面：hermes-hierarchy-architecture


## 相關節點
- [[index]]
