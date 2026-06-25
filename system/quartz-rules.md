---
status: active
title: "Quartz Rules"
summary: "Quartz Rules - 靜態網站部署與 GitHub Pages 發佈規範"
description: "Quartz 靜態網站部署與 GitHub Pages 發佈規範"
type: concept
tags: [quartz, deploy, flow]
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

## Frontmatter 格式問題（Quartz YAML 解析）

### 問題：`bad indentation of a mapping entry`

**原因**：Quartz 使用的 YAML parser 對 frontmatter 格式要求嚴格。

**常見錯誤模式**：

1. **summary 含特殊字元未加 quote**
   ```yaml
   # 錯誤
   summary: 2026-06-01 Summary：- Executed skill-name: completed task
   # 修正
   summary: "2026-06-01 Summary - Executed skill-name, completed task"
   ```

2. **summary 含逗號後接空格+小寫字母**
   ```yaml
   # 錯誤（Quartz 會誤判為 mapping entry）
   summary: "2026-06-01 Summary - Executed skill-name, completed backup of Hermes"
   # 修正：精簡內容或改用全形逗號
   summary: "2026-06-01 Summary - Executed skill-name, completed backup"
   ```

3. **frontmatter 內含 wikilink**
   ```yaml
   # 錯誤
   related: - [[page-name]]
   # 修正：將 wikilink 移至 body
   ```

4. **title / description 含冒號未加 quote**
   ```yaml
   # 錯誤
   ---
   title:Awesome DESIGN.md
   description: VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式
   summary: Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合
   ---
   # 修正
   ---
   title: "Awesome DESIGN.md"
   description: "VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式"
   summary: "Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合"
   ---
   ```

**規則**：
- title、description、summary 三個欄位**永遠用 double quote 包裹**
- summary 避免逗號後接空格+小寫字母的結構
- frontmatter 內禁止 wikilink、markdown 語法

**相關連結**

- [[schema]]：核心憲法
- [[policy]]：規則路由器
