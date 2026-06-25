---
status: active
title: "如何將 Obsidian 發佈為私人入口網站"
summary: "如何將 Obsidian 發佈為私人入口網站：推薦方案：Quartz + Cloudflare Zero Trust"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [obsidian, quartz, deploy]
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