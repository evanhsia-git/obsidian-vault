---
title: token-usage-dashboard
description: token-usage-dashboard — 概念說明頁面
summary: token-usage-dashboard
type: concept
status: active
priority: P2
tags: [hermes]
aliases: []
created: 2026-06-10
updated: 2026-06-10
date: 2026-06-10
publish: true
draft: false
related:
source:
due:
review:
---

# Token 監控面板方案 (Token Usage Dashboard)

本頁面記錄 Hermes Agent Token 使用量的監控解決方案，用於審計自動化任務成本與系統效率。

## 方案名稱：Hermes Agent Dashboard
- **官方來源**：[https://github.com/Bichev/hermes-dashboard](https://github.com/Bichev/hermes-dashboard)

## 核心價值
1. **透明化代理 (Transparent Proxy)**：作為 API 代理，即時攔截並記錄所有對 Anthropic API 的呼叫，提供精確的成本統計，而非事後估算。
2. **多維度分析**：
    - **成本拆分**：按模型、平台、日期拆解費用，精準監控每日 Cron Job 開銷。
    - **對話深度監測**：分析 Token 消耗分佈，有助於優化新聞推送 (daily-stock-news) 等任務的 Prompt 長度。
    - **系統健康指標**：整合 CPU、RAM、磁碟佔用與資料庫狀態，確保 VPS 自動化環境穩定。
3. **架構整合**：直接讀取 Hermes `state.db`，與現有知識體系無縫串接。

## 監控面板功能
- **KPI 面板**：每日/每月成本、Token 消耗量、請求總數。
- **分析圖表**：使用模型分佈、工具使用率、對話長度分佈與 API 錯誤率跟蹤。
- **Cron 管理**：直接查看已排程任務的運行狀態與歷史紀錄。

## 部署方式與檢視規範
- **檢視方式**：本應用為自架網頁程式，部署於 VPS 後，可透過任何瀏覽器遠端存取，無需本地端安裝軟體。
- **存取規範**：
    - **HTTPS (推薦)**：需網域指向 VPS IP 並開啟 80/443 連接埠以自動申請 SSL 憑證。
    - **HTTP (簡化)**：若無網域，可直接透過 `http://<VPS-IP>:<PORT>` 存取（需設定防火牆放行特定連接埠）。
- **安全性**：內建 Secret code 密碼驗證。
- **部署方式**：須於 VPS 自行部署，並非現成的第三方託管服務。

- [[openrouter-free-models]]
## 相關操作
- **每日 Token 成本報告檢查** (Job ID: `9c6ea63c1e5d`)：於每日 08:00 自動執行，若當日累計 Token 成本超過 $5.00 USD 閾值，系統將自動發送警戒通知。


## 相關節點
- [[index]]