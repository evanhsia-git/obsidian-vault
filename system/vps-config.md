---
title: "VPS 主機環境配置"
description: "VPS 主機環境配置 — 系統配置頁面"
summary: "VPS 主機環境配置"
type: schema
status: active
tags: [hermes]
created: 2026-06-07
updated: 2026-06-07
related:
---

# VPS 主機環境配置 (Linode Tokyo)

本系統運行於 Linode VPS，專為 Hermes Agent 與 Obsidian 知識庫運作優化。

## 主機規格

- **供應商**: Linode
- **區域**: JP, Tokyo 2
- **方案**: Linode 2 GB
- **CPU**: 1 Core
- **RAM**: 2 GB
- **儲存空間**: 50 GB Storage
- **Volumes**: 0

## 網路資訊

- **Public IPv4**: 172.104.105.71
- **Public IPv6**: 2400:8902::2000:c3ff:fe2f:98c1
- **加密狀態**: Encrypted

## 系統路徑與路徑配置

- **使用者家目錄**: `/root`
- **Hermes Agent 工作目錄**: `/usr/local/lib/hermes-agent`
- **Obsidian Vault 存放位置**: `/root/Documents/Obsidian Vault`
- **Quartz 發布目錄**: `/root/quartz`

---

## inode VPS 環境分析

| 項目             | 目前狀態               | 評估                 |
| -------------- | ------------------ | ------------------ |
| 作業系統           | Ubuntu 24.04.4 LTS | ✅ 最新 LTS           |
| CPU 架構         | x86\_64            | ✅ Intel/AMD 架構     |
| 記憶體            | 1.9 GB RAM         | ⚠️ 偏小              |
| 已使用 RAM        | 588 MB             | ✅ 正常               |
| 可用 RAM         | 約 1.3 GB           | ⚠️ 不足以跑大型 Electron |
| Swap           | 495 MB             | ⚠️ 太小              |
| Swap 已使用       | 321 MB             | 🚨 已開始吃 Swap       |
| 開機模式           | multi-user.target  | ❌ 無 GUI            |
| 桌面環境           | 無                  | ❌ 未安裝              |
| 顯示管理器          | 無                  | ❌ 未安裝              |
| X11 / Wayland  | 無                  | ❌ 未安裝              |
| Quartz         | 已運行                | ✅                  |
| Obsidian Vault | 已運行                | ✅                  |
| Nginx          | 推測已運行              | ✅                  |



# 最重要的結論

你的 VPS 是：

```
Ubuntu Server
+
純命令列模式
+
沒有桌面環境
```

從這裡可以確定：

## Hermes Desktop 無法顯示介面的主因

不是 CPU。

不是 x86\_64。

而是：

### 1. 沒有 GUI

你目前：

```
systemctl get-default
```

結果：

```
multi-user.target
```

代表：

```
CLI模式
(Command Line Interface)
```

而不是：

```
graphical.target
```

***

### 2. 沒有桌面環境

你的查詢：

```
dpkg -l | grep-E"xfce|gnome|kde|lxqt|mate"
```

完全沒有：

```
xfce4
gnome-shell
kde-plasma
lxqt
mate-desktop
```

代表：

```
沒有桌面
```

***

### 3. 記憶體其實也不夠

目前：

```
RAM = 1.9GB
```

而 Hermes Desktop 如果是 Electron 類程式：

通常啟動後：

| 項目             | RAM          |
| -------------- | ------------ |
| Ubuntu         | 500MB        |
| XFCE           | 300MB        |
| Chrome         | 500MB        |
| Hermes Desktop | 500MB\~1GB   |
| 合計             | 1.8GB\~2.5GB |

已經超出你的 VPS 容量。