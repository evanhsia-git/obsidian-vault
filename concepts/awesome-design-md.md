---
title:Awesome DESIGN.md
description: VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式，供 AI Agent 生成高品質 UI
summary: Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合，讓 AI 生成風格一致的 UI
type: concept
status: active
tags: [web-design, ai-ui, design-system, prompt-engineering, google-stitch]
created: 2026-06-24
updated: 2026-06-24
---

# Awesome DESIGN.md

[原始連結](https://github.com/VoltAgent/awesome-design-md)

## 核心概念

**DESIGN.md** 是 Google Stitch 引入的純文字設計系統格式：

- 讓 AI Agent 理解並複製專案視覺風格
- 不需 Figma 匯出或 JSON schema
- 直接放入專案根目錄，指示 AI「按照這個設計建頁面」即可
- [官方規範](https://stitch.withgoogle.com/docs/design-md/specification/)

## 架構層級

| 檔案 | 用途 |
|---|---|
| `AGENTS.md` | coding agents 如何建構專案 |
| `DESIGN.md` | design agents 如何感受專案外觀 |

## DESIGN.md 九大區塊

| # | 欄位 | 捕獲內容 |
|---|---|---|
| 1 | 視覺主題與氛圍 | 情緒、密度、設計哲學 |
| 2 | 色板與角色 | 語意色名 + hex code + 功能 |
| 3 | 字體規則 | font family + 層級表格 |
| 4 | 元件樣式 | buttons/cards/inputs/states |
| 5 | 佈局原則 | 間距/格線/留白 |
| 6 | 深度與高程 | shadow/表面層級 |
| 7 | 可做與不可做 | design guardrails |
| 8 | 響應式行為 | breakpoints/觸控/收合 |
| 9 | Agent 提示指南 | 快速色碼參考 + 即用提示 |

## 附加資產

- `preview.html` — 色碼、字體、按鈕、卡片視覺目錄（淺色）
- `preview-dark.html` — 暗色模式版本

## 分類（10 個領域）

- AI & LLM 平台
- 開發者工具/IDE
- 後端/資料庫/DevOps
- 生產力/SaaS
- 設計/創意工具
- 金融科技/加密
- 電商
- 媒體/消費科技
- 汽車
- 復古網頁

## 使用方式

1. 從集合中選擇符合美學的 `DESIGN.md`
2. 放到專案根目錄
3. 對 AI 說：「按照這個 DESIGN.md 讓我看看頁面」

## 價值

- 不需要 Figma access 也能生成風格一致的專業 UI
- 適用於快速 prototyping、風格遷移、樣式標準化
- MIT 開源，所有檔案都是公開可見的 CSS 值

##  REFERENCES

- [Google Stitch DESIGN.md 規範](https://stitch.withgoogle.com/docs/design-md/specification/)
- [awesome-design-md GitHub](https://github.com/VoltAgent/awesome-design-md)
- [Google Stitch](https://stitch.withgoogle.com/)
