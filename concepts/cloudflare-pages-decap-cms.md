---
title: Cloudflare Pages 運行 Decap CMS
description: Cloudflare Pages 運行 Decap CMS — 概念說明頁面
summary: Cloudflare Pages 運行 Decap CMS
type: concept
status: active
priority: P2
tags: []
aliases: []
created: 2026-06-12
updated: 2026-06-12
date: 2026-06-12
publish: true
draft: false
related:
source:
due:
review:
---

# Cloudflare Pages 運行 Decap CMS

## 🎯 核心概念

**Cloudflare Pages 運行 Decap CMS** 需要一個 **OAuth Proxy** 來處理 GitHub 認證流程，因為 GitHub OAuth 需要伺服器端組件。

## 🔧 解決方案架構

```
使用者瀏覽器 → Decap CMS → Cloudflare Worker (OAuth Proxy) → GitHub OAuth → 回傳 Token → Decap CMS
```

## 📋 實作步驟

### 1. 建立 GitHub OAuth App

1. 前往 GitHub **Settings** → **Developer Settings** → **OAuth Apps**
2. 建立新的 OAuth App
3. **Homepage URL**: 設定為您的網站 URL（例如 `https://indexedev.com`）
4. **Authorization callback URL**: 設定為您的網站 URL
5. GitHub 會提供 **Client ID**
6. 生成 **Client Secret**

### 2. 部署 Cloudflare Worker

使用 `decap-proxy` 專案：

```bash
# 克隆專案
git clone https://github.com/sterlingwes/decap-proxy.git
cd decap-proxy

# 安裝依賴
npm install

# 設定環境變數
wrangler secret put GITHUB_CLIENT_ID
wrangler secret put GITHUB_CLIENT_SECRET

# 部署
wrangler deploy
```

**環境變數說明**：
- `GITHUB_CLIENT_ID`: GitHub OAuth App 的 Client ID
- `GITHUB_CLIENT_SECRET`: GitHub OAuth App 的 Client Secret
- `GITHUB_REPO_PRIVATE`: 可選，預設 `false`，設定為 `1` 表示私有 repo

### 3. 整合專案檔案

**專案結構**：
```
your-project/
├── functions/
│   └── api/              # Cloudflare Functions
├── static/
│   └── admin/            # Decap CMS 前端
│       ├── index.html
│       └── config.yml
├── content/              # CMS 內容
└── public/               # 靜態輸出
```

**`static/admin/config.yml` 設定**：
```yaml
backend:
  name: github
  repo: username/repo
  branch: main
  base_url: https://your-worker.workers.dev
  auth_endpoint: auth  # 可選，預設為 auth

media_folder: "content/images"
public_folder: "/images"

collections:
  - name: "posts"
    label: "Posts"
    folder: "content/posts"
    create: true
    slug: "{{slug}}"
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Body", name: "body", widget: "markdown" }
```

### 4. 部署並存取

1. 發布網站至 Cloudflare Pages
2. 存取 Decap CMS 管理介面：`https://your-site.com/admin/`
3. 點擊「Login with GitHub」按鈕
4. 完成 OAuth 授權後進入 CMS

## 🔑 Cloudflare Worker 端點說明

### `/auth` 端點
- **功能**：啟動 GitHub OAuth 流程
- **回應**：302 重定向至 GitHub 授權頁面
- **參數**：`?provider=github`

### `/callback` 端點
- **功能**：處理 GitHub OAuth 回調
- **回應**：200 OK，包含 HTML 腳本
- **參數**：`?code=AUTHORIZATION_CODE`

## 📚 參考資源

- [Decap CMS 官方網站](https://www.decapcms.org)
- [Cloudflare Pages](https://pages.cloudflare.com)
- [GitHub OAuth Apps](https://github.com/settings/developers)
- [decap-proxy GitHub](https://github.com/sterlingwes/decap-proxy)
- [decap-cms-cloudflare-pages GitHub](https://github.com/SubhenduX/decap-cms-cloudflare-pages)

## ⚠️ 注意事項

1. **Worker URL 格式**：`base_url` 應包含完整域名，不包含尾隨斜線
2. **OAuth Scope**：預設使用 `repo,user` scope（私有 repo）或 `public_repo,user`（公開 repo）
3. **部署流程**：修改後需重新部署 Worker 才能生效
4. **CORS 設定**：Worker 自動處理 CORS，無需額外設定

## 🔄 與 Netlify 的差異

| 特性 | Netlify | Cloudflare Pages |
|------|---------|------------------|
| 認證服務 | 內建 Netlify Identity | 自訂 OAuth Proxy |
| GitHub API | Git Gateway | 直接 API 呼叫 |
| 部署平台 | Netlify | Cloudflare Pages |
| 成本 | 免費層有限制 | 免費層較寬鬆 |

---
## 相關節點
- [[keystonejs]]
- [[obsidian-cms]]