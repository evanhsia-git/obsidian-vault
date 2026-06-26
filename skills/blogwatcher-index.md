---
title: "blogwatcher-index"
description: "blogwatcher-index — 技能說明頁面"
summary: "blogwatcher-index"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

# Blogwatcher 工具與 RSS 監控機制

## 1. 摘要：Blogwatcher 是什麼？
Blogwatcher 是一套內建於 Hermes Agent 的終端機監控工具 (`blogwatcher-cli`)，專門用於監控部落格、RSS 或 Atom 餵送 (Feeds)。
* **原生整合**：無需額外部署 Miniflux 或 Docker 容器，直接在 VPS 上透過腳本即可運行。
* **狀態管理**：自動追蹤變更內容並存入本地環境，Agent 可隨時調用最新狀態，無需每次重新搜尋網頁。

## 2. 核心效益分析
| 比較項目 | 原本方式 (web_extract) | Blogwatcher (推薦方案) |
| :--- | :--- | :--- |
| **執行效率** | **低**：每次需搜尋、下載頁面、降噪清理。 | **極高**：僅抓取 XML 更新，解析速度極快。 |
| **Token 使用** | **高**：包含廣告、導覽列等雜訊。 | **極低**：僅處理新聞純文字內容。 |
| **運作複雜度** | **低**：無額外依賴。 | **極低**：內建於系統，原生指令集。 |
| **穩定性** | **中**：視網站結構與防火牆而定。 | **高**：直接抓取標準 RSS 格式。 |

## 3. 實作規範 (SCHEMA)
1. **導航**: 確認目標網站是否有 RSS/Atom Feed (可透過 RSSHub 轉換)。
2. **提取**: 使用 `blogwatcher-cli` 訂閱並同步內容。
3. **執行**: Agent 定時讀取本地監控紀錄，提取已變更的條目。
4. **沉澱**: 確保解析後的內容符合「標題：摘要：連結」格式，並寫入 log。

## 4. 維護原則
- **降噪處理**: 優先使用 Blogwatcher 的標準化解析輸出。
- **一致性**: 確保所有 RSS 來源統一加入 `blogwatcher-cli` 的訂閱清單，方便統一管理推送。


- [[wall-street-portfolio-manager]]
## 相關節點
- [[index]]