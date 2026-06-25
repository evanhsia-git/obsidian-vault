---
title: "Cloudflare 與 Hermes Agent 整合"
description: "Cloudflare 與 Hermes Agent 整合 — 概念說明頁面"
summary: "Cloudflare 與 Hermes Agent 整合"
type: concept
status: active
priority: P2
tags: []
aliases: []
created: 2026-06-07
updated: 2026-06-07
date: 2026-06-07
publish: true
draft: false
related:
source:
due:
review:
---

# Cloudflare 在本系統中的角色與價值

## 為何選擇 Cloudflare
* **全域 CDN & DDoS 防護**：在 335+ 城市提供低延遲服務，保護 VPS 不受大規模攻擊。
* **Zero‑Trust Access**：透過 Cloudflare Access 可在不開 VPN 的情況下，使用 Google/GitHub OAuth 進行 MFA 登入，適合保護 CollabMD 編輯介面。
* **Cloudflare Tunnel (Argo Tunnel)**：無需公開 IP，即可將本機服務（CollabMD、Quartz、Hermes Agent API）安全暴露於網路，避免直接暴露真正的 VPS IP。
* **Workers & AI 平台**：未來若需要在邊緣執行輕量 AI 推理或自動化任務，可直接在 Cloudflare Workers 中部署，減少 VPS 計算負載。

## 與 Hermes Agent 的直接幫助
| 功能 | Cloudflare 方案 | 對 Hermes Agent 的效益 |
| :--- | :--- | :--- |
| **DDoS & WAF 防禦** | Cloudflare 網路層防禦、WAF 內建規則 | 防止惡意流量導致 Agent 中斷或資源耗盡 |
| **Zero‑Trust 身分驗證** | Cloudflare Access + MFA | 只允許授權使用者存取 CollabMD 編輯介面與 Agent API，提升安全性 |
| **安全隧道 (Tunnel)** | cloudflared Argo Tunnel | 隱藏真實 VPS IP，免除防火牆開放 22/80/443，減少被掃描風險 |
| **全域 CDN 加速** | Cloudflare CDN | 靜態資源（如 Quartz 產出的網站）可快速全球分發，提升訪問體驗 |
| **Workers/AI 執行** | Cloudflare Workers、Workers AI | 可把部分重度計算（例如快速文字摘要、簡易模型推論）搬到邊緣，降低 VPS CPU 負載 |

## 部署建議流程
1. **安裝 & 設定 cloudflared**：`apt-get install cloudflared`，建立 `cloudflared tunnel create hermes-tunnel`，產生 `tunnel.yml` 指向本機 8080（CollabMD）與 3000（Hermes API）。
2. **設定 Cloudflare Access**：於 Cloudflare Dashboard → Zero Trust → Access → Applications，新增 CollabMD 網址，選擇 Google/GitHub OAuth，啟用 MFA。
3. **啟用 CDN**：將 Quartz 產出的網站部署至 Cloudflare Pages，使用自動化 CI 推送即可享受全域加速。
4. **未來擴充**：若有 AI 任務需求，編寫 Workers 程式（`wrangler init`），使用 Workers AI 端點呼叫模型；在 Agent 中以 HTTP 呼叫 Workers 取得結果。

## 結論
* **安全**：Zero‑Trust + Tunnel 為 VPS 提供免曝露 IP 的防護層。
* **效能**：CDN 與 Workers 讓靜態內容與輕量運算更快、更省資源。
* **彈性**：可在不改變現有 CollabMD、Quartz 流程的前提下，逐步導入 Cloudflare 服務。

---

相關連結：
- [[concepts/private-website-access|私人網站訪問]]
- [[concepts/vps-obsidian-blog-deployment|VPS Obsidian 與部落格部署方案]]

相關頁面：相關頁面：## 相關節點
- [[index]]
