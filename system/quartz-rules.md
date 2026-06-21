---
title: Quartz Rules
summary: Quartz Rules：**Quartz Rules**
description: Quartz 靜態網站部署與 GitHub Pages 發佈規範
type: concept
tags: [quartz, github, deploy, rules]
created: 2026-06-21
updated: 2026-06-21
---

**Quartz Rules**

發布網站前必須遵守。

**技術棧**

- 框架：Quartz 5（flowershow）
- 部署：GitHub Pages
- CI/CD：GitHub Actions

**目錄結構**

- Obsidian Vault → Quartz content/
- 設定檔：quartz.config.ts
- 元件：quartz/components/

**部署流程**

1. 同步 Obsidian Vault → Quartz content/
2. 執行 `npx quartz build`
3. 推送至 GitHub Pages
4. 驗證部署結果

**安全規範**

- 敏感檔案（.env、API Key）禁止出現在 content/ 同步範圍
- .gitignore 排除：database/、scripts/、util/、temp/、ivan-notes/

**相關連結**

- [[schema]]：核心憲法
- [[policy]]：規則路由器
