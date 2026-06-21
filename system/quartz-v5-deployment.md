---
title: Quartz v5 GitHub Pages 部署
description: Quartz v5 GitHub Pages 部署 — 系統配置頁面
summary: Quartz v5 GitHub Pages 部署
type: system
status: published
priority: P2
tags: [maintenance, setup, hermes, obsidian]
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

# Quartz v5 網站部署架構

本系統的 Obsidian 知識庫透過 **Quartz v5.0.0** 發布為靜態網站，採 GitHub Actions 自動部署至 GitHub Pages。

## 線上網址

- 網站： https://evanhsia-git.github.io/quartz-site/
- Repo： https://github.com/evanhsia-git/quartz-site （公開）

## 架構重點

- **程式碼 + 內容同 repo**：Quartz v5 官方做法，筆記放在 `content/` 資料夾，與 Quartz 程式本體在同一 repo（取代舊的純內容 repo）。
- **本機原始碼**：`/root/quartz`（已是 v5.0.0，git tag `v5.0.0-237`）。
- **部署觸發**：push 到 `main` 分支 → GitHub Actions 自動 `npx quartz build` → 部署至 GitHub Pages。

## 部署 workflow（`.github/workflows/deploy.yml`）

官方 GitHub Pages 標準流程：`actions/checkout` → `setup-node@22` → `npm ci` → `npx quartz build` → `upload-pages-artifact` → `deploy-pages@v4`。權限需 `pages: write` 與 `id-token: write`。

## 2026-06-11 部署紀錄與踩雷

1. **舊 repo `evanhsia-git/obsidian-vault` 的部署是壞的**：其 `deploy.yml` 用 `npm add quartz@0.0.1`——這是 npm 上一個不相干的廢棄套件，並非 jackyzha0 的 Quartz，故根本無法建置。Quartz 正確安裝方式是 git clone 整個 repo，不是 npm install。
2. **敏感檔處理**：新 repo 建立時已排除 `performance_monitoring.db`。`Environment Keys.md` 內金鑰已 [REDACTED]，無明碼洩漏。
3. **PAT 缺 `workflow` scope**：git push 與 gh API 都會拒絕含 `.github/workflows/` 的變更（HTTP 404 / remote rejected）。本次以「網頁手動新增 workflow 檔」繞過。
4. **git 認證**：需先 `gh auth setup-git` 讓 git push 帶上 token。

## 如何為 PAT 補上 workflow scope

未來若要從終端機直接推送 workflow 變更，需讓 token 具備 `workflow` scope：

- **最快**：`gh auth refresh -s workflow`（互動授權，瀏覽器貼一次性代碼）
- **手動**：GitHub Settings → Developer settings → Personal access tokens → 編輯該 token → 勾選 `workflow` → 更新
- **驗證**：`gh auth status`（看 Token scopes 是否含 workflow）

### ✅ 2026-06-11 已驗證可用

使用者已於網頁手動為 PAT 開啟 `workflow` scope。實測結果：

- `gh auth status` Token scopes：`repo`, `workflow`, `read:org`, `admin:public_key` ✓
- 終端機直接 `git push` 含 `.github/workflows/` 的 commit（`ffc425d..4599b25`）：成功，不再被拒
- 推送自動觸發 GitHub Actions 部署：`in_progress` → success ✓

結論：**日後可直接在終端機修改 workflow 並 push，無需再用網頁手動加檔。**

## 相關連結

- [[vps-config]]
- [[index]]
