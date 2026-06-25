---
status: active
title: "私人網站訪問"
summary: "私人網站訪問：1. GitHub Pages 私人權限 (適用於企業/團隊帳號)"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [obsidian, quartz, deploy]
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