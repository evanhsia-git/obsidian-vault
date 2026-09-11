---
title: "mklab-stock 安全刪除指南"
description: "列出 mklab-stock 倉庫中可安全刪除的備份/測試檔案，不影響網站運作"
summary: "提供 mklab-stock 倉庫清理指南，識別哪些檔案屬於備份、測試或開發副本，可放心刪除以保持倉庫乾淨"
type: concept
tags: [mklab-stock, cleanup, maintenance, obsidian]
status: active
created: 2026-09-11
updated: 2026-09-11
---

# mklab-stock 安全刪除指南

本頁面列出在 `/root/mklab-stock` 倉庫中，**可以安全刪除** 的檔案與資料夾。刪除這些檔案將不會影響 mklab-stock 網站的正常運作（首頁 `index.html` 及其相依的資料、腳本、樣式等）。

## 🗑️ 可安全刪除的檔案類別

### 1. 備份檔案（任何具有以下副檔名的檔案）
- `.backup`, `.bak`, `.orig`, `.new`, `.save`
- 範例：
  - `assets/css/component.css.save`
  - `index.html.save`, `index.html.before_fix`
  - `.github/workflows/daily-update.yml.backup*`（所有備份版本）
  - `skills/data/fetch_data.py.backup*`
  - `skills/qa-gate/validate_data.py.backup`
  - `data/stocks.json.backup_industry_test`
  - `data/stocks.test.json`

### 2. 測試／實驗 HTML 頁面
- 非首頁或儀表板所必需的功能展示頁：
  - `mklab-stock-backtest.html`
  - `mklab-stock-breadth.html`
  - `mklab-stock-compare.html`
  - `mklab-stock-digest.html`
  - `mklab-stock-dividend.html`
  - `mklab-stock-help.html`
  - `mklab-stock-log.html`
  - `mklab-stock-portfolio.html`
  - `mklab-stock-research.html`
  - `mklab-stock-screener.html`
  - `mklab-stock-watchlist.html`

### 3. Hermes 超能力工具內部進度紀錄
- 整個目錄：`.superpowers/`
  - 用於追蹤任務進度的本機檔案，網站不會讀取

### 4. Python 快取目錄
- `skills/data/__pycache__/`
  - 執行時會自動重新產生，可安全刪除

### 5. 文件／說明（純參考，網站不直接使用）
- 整個目錄：`docs/`
- `README.md`
- `HANDOFF.md`
- `skills/README.md`
- 各技能目錄下的 `skill.md` 檔案
  - 僅供開發者參考，網站運行不需要

### 6. GitHub Actions 工作流程備份
- 除了 `daily-update.yml`（若要保留自動更新功能）外，所有其他備份工作流程：
  - `.github/workflows/qa-gate.yml`
  - `.github/workflows/html-health.yml`
  - 任何具有 `.backup`, `.bak`, `.orig`, `.new` 等副檔名的工作流程檔案

## ✅ 必須保留的核心檔案（網站運作所需）

為確保網站正常運作，**請務必保留** 以下目錄與檔案：
- `index.html`（首頁入口）
- `assets/`（`css/`、`js/` 目錄）
- `data/`（`latest_prices.csv`、`stocks.json`、`markets.json`、`industry.json`、`industry-codes.json`、`etf-list.json`、`etf-classification.json`、`digest/index.json`、`history/` 內的每日歷史檔——若網站需要顯示歷史趨勢則保留）
- `scripts/`、`styles/`（若專案有使用）
- `vendor/`（`lightweight-charts.min.js` 第三方 K 線圖表庫）
- `templates/`（若網站使用了模板引擎）
- `rss.xml`（若有提供 RSS 訂閱）

## 📋 快速清理指令（執行前請先備份或使用 `git stash`）

```bash
cd /root/mklab-stock

# 1. 刪除所有備份檔案
find . -type f \( -name "*.backup" -o -name "*.bak" -o -name "*.orig" -o -name "*.new" -o -name "*.save" \) -delete

# 2. 刪除測試用 HTML 頁面（僅保留 index.html）
rm -f mklab-stock-{backtest,breadth,compare,digest,dividend,help,log,portfolio,research,screener,watchlist}.html

# 3. 刪除 .superpowers 目錄
rm -rf .superpowers

# 4. 刪除 Python 快取
rm -rf skills/data/__pycache__

# 5. （可選）刪除文件與說明檔
rm -rf docs
rm -f README.md HANDOFF.md

# 6. 刪除 GitHub Actions 備份工作流程（保留 daily-update.yml 若要自動更新）
find .github/workflows -type f -name "*.backup" -o -name "*.bak*" -o -name "*.orig" -o -name "*.new" -delete
```

> **執行前建議**：先執行 `git status` 或 `git diff --check` 確認沒有誤刪到真正需要的檔案；亦可先 `git stash` 備份目前的工作狀態。

## 🔗 相關知識
- [[mklab-stock 倉庫結構]]
- [[Obsidian Vault 維護指南]]
- [[GitHub Actions 自動化工作流程]]

---

[LOG]
已於 2026-09-11 建立此頁面，記錄 mklab-stock 倉庫中可安全刪除的檔案清單，以便未來參考與倉庫維護。