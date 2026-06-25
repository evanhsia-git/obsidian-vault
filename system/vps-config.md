---
title: "VPS 主機環境配置"
description: "VPS 主機環境配置 — 系統配置頁面"
summary: "VPS 主機環境配置"
type: schema
status: active
priority: P2
tags: [hermes]
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

# VPS 主機環境配置 (Linode Tokyo)

本系統運行於 Linode VPS，專為 Hermes Agent 與 Obsidian 知識庫運作優化。

## 主機規格
* **供應商**: Linode
* **區域**: JP, Tokyo 2
* **方案**: Linode 2 GB
* **CPU**: 1 Core
* **RAM**: 2 GB
* **儲存空間**: 50 GB Storage
* **Volumes**: 0

## 網路資訊
* **Public IPv4**: 172.104.105.71
* **Public IPv6**: 2400:8902::2000:c3ff:fe2f:98c1
* **加密狀態**: Encrypted

## 系統路徑與路徑配置
* **使用者家目錄**: `/root`
* **Hermes Agent 工作目錄**: `/usr/local/lib/hermes-agent`
* **Obsidian Vault 存放位置**: `/root/Documents/Obsidian Vault`
* **Quartz 發布目錄**: `/root/quartz`

## 備註
此主機目前負責運行 Hermes Agent 的核心邏輯、Quartz 靜態網站生成與 CollabMD 協作介面。

- [[backup-rules]]
相關頁面：相關頁面：## 相關節點
- [[index]]