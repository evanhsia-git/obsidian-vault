---
title: VPS Obsidian 與部落格部署方案
description: VPS Obsidian 與部落格部署方案 — 概念說明頁面
summary: VPS Obsidian 與部落格部署方案
type: concept
status: active
priority: P2
tags: ["concept", "obsidian"]
aliases: []
created: 2026-06-06
updated: 2026-06-06
date: 2026-06-06
publish: true
draft: false
related:
source:
due:
review:
---

# VPS Obsidian 與部落格部署方案

本頁面記錄在 Linode VPS 上部署 Obsidian 網頁編輯環境與自動化部落格發佈系統的規劃。

## 執行目標
1. **網頁編輯**：部署 CollabMD 供網頁端編輯本地 Obsidian Vault。
2. **動態發佈**：部署 Hugo 作為部落格系統，結合 Docker 進行自動化建置。
3. **自動化流程**：
   - 使用者在網頁端編輯 (`CollabMD`) -> 觸發 `inotify` 或定時同步 -> `Hugo` 自動編譯 -> `Nginx` 發佈。

## 部署階段規劃
- [ ] **Phase 1: 環境整備**
  - 確認 Docker 與 Nginx 安裝狀態。
  - 設定 VPS 防火牆規則。
- [ ] **Phase 2: CollabMD 部署**
  - 建立 CollabMD 容器化部署環境。
  - 掛載 `/root/Documents/Obsidian Vault` 進行本地同步。
- [ ] **Phase 3: Hugo 發佈系統**
  - 初始化 Hugo 專案目錄。
  - 撰寫 `Dockerfile` 以容器化 Hugo。
  - 串接發佈流程 (CI/CD 腳本)。

## 相關頁面
- [[concepts/obsidian-web-editing-solutions|在網路上編輯 Obsidian 筆記]]
- [[concepts/obsidian-website-deployment|如何發佈私人入口網站]]

相關頁面：相關頁面：相關頁面：[[obsidian-web-editing-solutions]]

相關頁面：[[obsidian-wiki-conventions]]

相關頁面：[[obsidian-website-deployment]]

## 相關節點
- [[index]]
