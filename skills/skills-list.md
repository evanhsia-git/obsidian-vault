---
title: "Skills List"
description: "所有已安裝 Skills 清單（原廠內建 + 使用者自建）"
summary: "skills-list — 原廠內建 + 使用者自建 Skills 完整目錄"
type: index
status: active
tags: [agent, hermes, knowledge]
created: 2026-06-08
updated: 2026-07-11
---

# Skills List（2026-07-11 更新）

> **2026-07-11 維護紀錄**：
> - 清理 4 個無爭議項目：2 個備份副本（`twse-stock-data.bak_*`、`tw-stock-data-enrichment.bak_*`）+ 2 個 DEPRECATED（`daily-news-stock-market`、`quartz-static-site-generator`），已歸檔至 `/root/backups/skills-removed-20260711/`。
> - `skills-list` 技能強化：**執行前必須用 clarify 詢問 A/B/C（使用者自建/原廠內建/全部），未獲回應自動選 A**。
> - `cron-list` 技能輸出改為**統一表格範本**（# / Job 名稱 / 技能 / 腳本路徑 / 模式），詳見 [[skills/cron-list|Cron List]]。
> - 總數：原廠內建 135 + 使用者自建 59（清理後）。

## 原廠內建 Skills

> 以下以檔案系統實際技能為準（2026-07-11 對比刷新）：原廠內建共 145 個 / 22 分類。

### ai
- ai-hallucination-suppression — Comprehensive framework for suppressing AI hallucinations...

### automation
- news-aggregator-pattern — Standardized workflow for building autonomous news-collec...

### autonomous-ai-agents
- agent-self-evolution — Guide to installing, configuring, and using the Hermes Ag...
- claude-code — Delegate coding to Claude Code CLI (features, PRs).
- codex — Delegate coding to OpenAI Codex CLI (features, PRs).
- hermes-agent — Configure, extend, or contribute to Hermes Agent.
- kanban-codex-lane — Use when a Hermes Kanban worker wants to run Codex CLI as...
- opencode — Delegate coding to OpenCode CLI (features, PR review).

### creative
- agent-architecture-builder — Generate architecture diagrams that illustrate the intera...
- architecture-diagram — Dark-themed SVG architecture/cloud/infra diagrams as HTML.
- ascii-art — ASCII art: pyfiglet, cowsay, boxes, image-to-ascii.
- ascii-video — ASCII video: convert video/audio to colored ASCII MP4/GIF.
- baoyu-article-illustrator — Article illustrations: type × style × palette consistency.
- baoyu-comic — Knowledge comics (知识漫画): educational, biography, tutorial.
- baoyu-infographic — Infographics: 21 layouts x 21 styles (信息图, 可视化).
- claude-design — Design one-off HTML artifacts (landing, deck, prototype).
- comfyui — Generate images, video, and audio with ComfyUI — install,...
- creative-ideation — Generate project ideas via creative constraints.
- design-md — Author/validate/export Google's DESIGN.md token spec files.
- excalidraw — Hand-drawn Excalidraw JSON diagrams (arch, flow, seq).
- humanizer — Humanize text: strip AI-isms and add real voice.
- manim-video — Manim CE animations: 3Blue1Brown math/algo videos.
- mermaid-diagram-template — 儲存與管理常用的 Mermaid 流程圖範本，用於自動化生成清晰的工作流圖表。
- p5js — p5.js sketches: gen art, shaders, interactive, 3D.
- pixel-art — Pixel art w/ era palettes (NES, Game Boy, PICO-8).
- popular-web-designs — 54 real design systems (Stripe, Linear, Vercel) as HTML/CSS.
- pretext — Use when building creative browser demos with @chenglou/p...
- sketch — Throwaway HTML mockups: 2-3 design variants to compare.
- songwriting-and-ai-music — Songwriting craft and Suno AI music prompts.
- touchdesigner-mcp — Control a running TouchDesigner instance via twozero MCP ...

### data-science
- backtest-visualizer — Portfolio and strategy backtesting visualization system
- chart-generator — Generate institutional-grade financial charts and visuali...
- financial-dashboard — Generate interactive institutional financial dashboards
- hermes-wallstreet-agent — >
- jupyter-live-kernel — Iterative Python via live Jupyter kernel (hamelnb).
- pdf-report-pitfalls — >
- python-quant-finance — Python quantitative finance stack — market data, backtest...
- wall-street-portfolio-manager — >
- wall-street-portfolio-manager-pdf — >

### devops
- decap-cms-setup — Setup Decap CMS (formerly Netlify CMS) on a static site h...
- devops-cloudflare-integration — A reusable workflow for securing and speeding up a VPS-ho...
- filesystem-hygiene-standardization — Workflow for eliminating file sprawl and enforcing absolu...
- get-news-resources — 獲取、驗證並持續優化財經新聞資源來源的標準工作流程。
- hermes-cron-maintenance — Managing and troubleshooting Hermes Cron jobs, specifical...
- kanban-orchestrator — Decomposition playbook + anti-temptation rules for an orc...
- kanban-worker — Pitfalls, examples, and edge cases for Hermes Kanban work...
- nginx-webdav-setup — Nginx WebDAV 伺服器設定參考，包含路徑空格處理、root vs alias 差異、權限問題排查
- openbb-finance — OpenBB 金融資料平台使用指南。透過 OpenBB Python API 獲取全球股市、台股、ETF、指數、外...

### email
- himalaya — Himalaya CLI: IMAP/SMTP email from terminal.

### finance
- compact-equity-research — 產生精簡版機構風格個股研究報告：500–600字、A4兩頁內、重點數據表、可依需求輸出 Markdown/HTML...
- daily-stock-picker — 當需要從前 N 大市值股票進行量化評分選股、每日推薦短/中/長期投資標的、計算多因子評分（PE/PB/殖利率/RO...
- equity-research-premium — 生成頂級機構級個股深度分析報告 v9.9。優化 A4 頁面佈局，採用兩頁滿版設計，並強化量化指標呈現與購買建議視覺化。
- equity-research-text — 專為 Telegram 優化的高密度文字個股分析報告。採用 v2.0-Interactive 模版，將深度量化分析...
- finance-report-template — Template for generating institutional-grade equity resear...
- institutional-equity-report — Generate ultra-high-fidelity, institutional-grade equity ...

### gaming
- minecraft-modpack-server — Host modded Minecraft servers (CurseForge, Modrinth).
- pokemon-player — Play Pokemon via headless emulator + RAM reads.

### github
- codebase-inspection — Inspect codebases w/ pygount: LOC, languages, ratios.
- github-auth — GitHub auth setup: HTTPS tokens, SSH keys, gh CLI login.
- github-code-review — Review PRs: diffs, inline comments via gh or REST.
- github-issues — Create, triage, label, assign GitHub issues via gh or REST.
- github-pr-workflow — GitHub PR lifecycle: branch, commit, open, CI, merge.
- github-repo-management — Clone/create/fork repos; manage remotes, releases.

### mcp
- native-mcp — MCP client: connect servers, register tools (stdio/HTTP).

### media
- gif-search — Search/download GIFs from Tenor via curl + jq.
- heartmula — HeartMuLa: Suno-like song generation from lyrics + tags.
- songsee — Audio spectrograms/features (mel, chroma, MFCC) via CLI.
- spotify — Spotify: play, search, queue, manage playlists and devices.
- youtube-content — YouTube transcripts to summaries, threads, blogs.

### mlops
- huggingface-hub — HuggingFace hf CLI: search/download/upload models, datasets.

### note-taking
- llm-wiki-workflow — >
- obsidian-maintenance-automation — Automated workflows for structural integrity and taxonomy...
- obsidian — Read, search, create, and edit notes in the Obsidian vault.
- obsidian-structural-maintenance — Bulk renaming, tag normalization, and orphan resolution f...
- obsidian-vault-compliance — Ensures all operations within the Obsidian Vault (file cr...
- obsidian-vault-health-monitor — Comprehensive health monitoring and lint checking for Obs...
- obsidian-webdav-sync — 設定 Obsidian 透過 WebDAV 與自建 Nginx 伺服器同步。涵蓋 Nginx WebDAV 配置、...
- obsidian-wiki — 管理 Obsidian Wiki、Wiki-LLM、知識圖譜、Quartz 與 Vault 維護

### obsidian
- obsidian-dataview-dashboards — >

### productivity
- airtable — Airtable REST API via curl. Records CRUD, filters, upserts.
- communication-protocol — Standardized communication protocols for Agent-User inter...
- communication-style — Defines user preferences for Telegram communication: Trad...
- etf-comparison-tool — Build and send interactive ETF holdings comparison HTML t...
- google-workspace — Gmail, Calendar, Drive, Docs, Sheets via gws CLI or Python.
- linear — Linear: manage issues, projects, teams via GraphQL + curl.
- maps — Geocode, POIs, routes, timezones via OpenStreetMap/OSRM.
- markdown-link-formatting — Standardize markdown link output for this user – title in...
- nano-pdf — Edit PDF text/typos/titles via nano-pdf CLI (NL prompts).
- notion-api-update — Update Notion databases with structured data using Notion...
- notion — Notion API via curl: pages, databases, blocks, search.
- notion-sync — Synchronize local directory structures, files, and conten...
- obsidian-to-html — 專業級 Obsidian Markdown 轉 HTML 工具。使用 Tailwind CSS 與 Typogra...
- ocr-and-documents — Extract text from PDFs/scans (pymupdf, marker-pdf).
- petdex — Install and select animated petdex mascots for Hermes.
- powerpoint — Create, read, edit .pptx decks, slides, notes, templates.
- teams-meeting-pipeline — Operate the Teams meeting summary pipeline via Hermes CLI...
- telegram-function-button — 當使用者輸入「功能」時，產生一個 Telegram 互動式功能按鈕選單。點擊按鈕後會根據對應的 callback_...
- telegram-interactive-ui — 實現 Telegram 互動式 UI (Inline Keyboards) 與即時狀態更新的整合方案。涵蓋從發送按...
- telegram-message-file-sender — 使用 Telegram Bot API (curl) 傳送檔案。嚴禁使用 send_message 的 MEDIA...
- vision-to-notion — Extract structured tabular data from an image using visio...

### red-teaming
- godmode — Jailbreak LLMs: Parseltongue, GODMODE, ULTRAPLINIAN.

### research
- arxiv — Search arXiv papers by keyword, author, category, or ID.
- blogwatcher — Monitor blogs and RSS/Atom feeds via blogwatcher-cli tool.
- cnn-fear-and-greed-api — Python script for fetching CNN Fear & Greed Index data vi...
- financial-news-gathering — Retrieve financial market news and index data from source...
- llm-wiki — Karpathy's LLM Wiki: build/query interlinked markdown KB.
- polymarket — Query Polymarket: markets, prices, orderbooks, history.
- research-paper-writing — Write ML papers for NeurIPS/ICML/ICLR: design→submit.
- taiwan-stock-market-news — Gather Taiwan stock market news, TAIEX, KOSPI, and TOPIX ...
- web-research-validated-links — Web research workflow that collects articles/news from se...

### smart-home
- openhue — Control Philips Hue lights, scenes, rooms via OpenHue CLI.

### social-media
- xurl — X/Twitter via xurl CLI: post, search, DM, media, v2 API.

### software-development
- debugging-hermes-tui-commands — Debug Hermes TUI slash commands: Python, gateway, Ink UI.
- git-branch-review — When asked to do a final whole-branch review, verify merg...
- hermes-agent-skill-authoring — Author in-repo SKILL.md: frontmatter, validator, structure.
- hermes-s6-container-supervision — Modify, debug, or extend the s6-overlay supervision tree ...
- node-inspect-debugger — Debug Node.js via --inspect + Chrome DevTools Protocol CLI.
- obsidian-quartz-maintenance — 專為 Obsidian Vault (Quartz 5) 設計的部署、修復與維護工作流
- plan — Plan mode: write an actionable markdown plan to .hermes/p...
- python-debugpy — Debug Python: pdb REPL + debugpy remote (DAP).
- quartz-deployment-and-maintenance — Specialized skill for deploying, troubleshooting, and mai...
- quartz-deployment-maintenance — Workflow for syncing Obsidian Vault to Quartz 5 and deplo...
- quartz-import-fix — Fix Quartz build error due to missing export of CustomOgI...
- requesting-code-review — Pre-commit review: security scan, quality gates, auto-fix.
- simplify-code — Parallel 3-agent cleanup of recent code changes.
- spike — Throwaway experiments to validate an idea before build.
- subagent-driven-development — Execute plans via delegate_task subagents (2-stage review).
- systematic-debugging — 4-phase root cause debugging: understand bugs before fixing.
- test-driven-development — TDD: enforce RED-GREEN-REFACTOR, tests before code.
- us-stock-news-summary — Guidelines for generating concise, representative daily U...
- writing-plans — Write implementation plans: bite-sized tasks, paths, code.

### superpowers
- brainstorming — 在開始任何創意工作前必須使用——建立功能、構建元件、新增功能、或修改行為。在實作前探索使用者意圖、需求與設計
- dispatching-parallel-agents — 當面對 2+ 個獨立任務、可同時進行而無共享狀態或順序依賴時使用
- executing-plans — 當有已撰寫的實作計畫需要在獨立 session 中執行並設有審查檢查點時使用
- finishing-a-development-branch — 當實作完成、所有測試通過、需要決定如何整合工作時使用——提供 merge、PR、或清理等結構化選項
- receiving-code-review — 當收到 code review 回饋、在實作建議前使用，特別是當回饋不清楚或技術上有疑問時——需要技術嚴謹與驗證，...
- requesting-code-review — 當完成任務、實作主要功能、或 merge 前需要驗證工作是否符合需求時使用
- subagent-driven-development — 當在 session 中執行有多個獨立 task 的實作計畫時使用
- systematic-debugging — 當遇到任何 bug、測試失敗、或異常行為時，在提出修復前使用
- test-driven-development — 當實作任何功能或 bugfix 時，在寫實作程式碼前使用
- using-git-worktrees — 當開始需要隔離的功能工作前，或執行實作計畫前使用——確保透過原生工具或 git worktree fallback...
- using-superpowers — 在開始任何對話時使用——建立如何找到並使用 skills 的規則，要求在任何回應前必須先 invoke skill
- verification-before-completion — 當即將聲稱工作完成、修復、或通過時，在 commit 或建立 PR 前使用——需要執行驗證命令並確認輸出，才能做出...
- writing-plans — 當有規格或需求需要轉為多步驟任務時，在碰觸程式碼前使用
- writing-skills — 當建立新 skill、編輯現有 skill、或部署前驗證 skill 是否正常時使用

## 使用者自建 Skills

### user/apple
- apple-notes — Apple Notes 管理
  - 位置: `user/apple/apple-notes/SKILL.md`
- apple-reminders — Apple Reminders 管理
  - 位置: `user/apple/apple-reminders/SKILL.md`
- findmy — Apple 裝置追蹤
  - 位置: `user/apple/findmy/SKILL.md`
- imessage — iMessage/SMS 收發
  - 位置: `user/apple/imessage/SKILL.md`
- macos-computer-use — macOS 螢幕操作
  - 位置: `user/apple/macos-computer-use/SKILL.md`

### user
- cron-list — Cron Job 列表格式（**2026-07-11 改為統一表格範本**：# / Job 名稱 / 技能 / 腳本路徑 / 模式）
  - 位置: `user/cron-list/SKILL.md`
- daily-news-technology — AI/科技新聞
  - 位置: `user/daily-news-technology/SKILL.md`
- daily-news-twstock — 台股新聞
  - 位置: `user/daily-news-twstock/SKILL.md`
- daily-news-unified — 整合新聞
  - 位置: `user/daily-news-unified/SKILL.md`
- daily-news-usstock — 美股新聞
  - 位置: `user/daily-news-usstock/SKILL.md`
- daily-stock-news — 股市指標（取代已刪除的 daily-news-stock-market）
  - 位置: `user/daily-stock-news/SKILL.md`
- dogfood — 網頁 QA
  - 位置: `user/dogfood/SKILL.md`
- finmind-api-troubleshooting — FinMind API 除錯
  - 位置: `user/finmind-api-troubleshooting/SKILL.md`
- h-memory-optimizer — 記憶體最佳化
  - 位置: `user/h-memory-optimizer/SKILL.md`
- hermes-agent-backup — Hermes 備份
  - 位置: `user/hermes-agent-backup/SKILL.md`
- hermes-agent-notion-sync — Notion 同步
  - 位置: `user/hermes-agent-notion-sync/SKILL.md`
- hermes-api-key-management — API key 管理
  - 位置: `user/hermes-api-key-management/SKILL.md`
- hermes-attestation-guardian — 安全認證
  - 位置: `user/hermes-attestation-guardian/SKILL.md`
- hermes-config-review — 配置審查
  - 位置: `user/hermes-config-review/SKILL.md`
- hermes-traffic-guardian — 流量監控
  - 位置: `user/hermes-traffic-guardian/SKILL.md`
- news-display-unifier — 新聞顯示統一
  - 位置: `user/news-display-unifier/SKILL.md`
- news-reporting-standards — 新聞報告標準
  - 位置: `user/news-reporting-standards/SKILL.md`
- notion-integration-patterns — Notion 整合模式
  - 位置: `user/notion-integration-patterns/SKILL.md`
- obsidian-lint — Vault 健康檢查
  - 位置: `user/obsidian-lint/SKILL.md`
- obsidian-quartz-deploy — Quartz 部署
  - 位置: `user/obsidian-quartz-deploy/SKILL.md`
- open-webui-setup — Open WebUI
  - 位置: `user/open-webui-setup/SKILL.md`
- quartz-static-site-deployment — Quartz 靜態站部署
  - 位置: `user/quartz-static-site-deployment/SKILL.md`
- safe-file-operations — 安全檔案操作
  - 位置: `user/safe-file-operations/SKILL.md`
- skills-list — Skills 清單（**2026-07-11 強化：執行前必須 clarify 詢問 A/B/C，預設 A**）
  - 位置: `user/skills-list/SKILL.md`
- sqlite-db-maintenance — SQLite 維護
  - 位置: `user/sqlite-db-maintenance/SKILL.md`
- stock-sentiment-analysis — 台股情緒分析
  - 位置: `user/stock-sentiment-analysis/SKILL.md`
- system-capability-audit — 系統能力稽核
  - 位置: `user/system-capability-audit/SKILL.md`
- taiwan-stock-news — 台股新聞
  - 位置: `user/taiwan-stock-news/SKILL.md`
- task-skill-loop — 任務技能循環
  - 位置: `user/task-skill-loop/SKILL.md`
- token-usage — Token 用量報告
  - 位置: `user/token-usage/SKILL.md`
- tpex-data-fetch — TPEX 資料抓取
  - 位置: `user/tpex-data-fetch/SKILL.md`
- twse-stock-data — TWSE 股票資料
  - 位置: `user/twse-stock-data/SKILL.md`
- tw-stock-data-enrichment — 台股資料補齊
  - 位置: `user/tw-stock-data-enrichment/SKILL.md`
- user-backup — 系統備份
  - 位置: `user/user-backup/SKILL.md`
- user-ivan-etf-sync — Ivan ETF 同步
  - 位置: `user/user-ivan-etf-sync/SKILL.md`
- user-maintenance — 系統維護
  - 位置: `user/user-maintenance/SKILL.md`
- usstock-content-cleaner — 美股新聞清理
  - 位置: `user/usstock-content-cleaner/SKILL.md`
- webhook-subscriptions — Webhook 事件驅動
  - 位置: `user/webhook-subscriptions/SKILL.md`
- wordpress-mcp-setup — WordPress MCP 連接
  - 位置: `user/wordpress-mcp-setup/SKILL.md`
- ws-data-analyzer — 數據分析
  - 位置: `user/ws-data-analyzer/SKILL.md`
- ws-data-gatherer — 數據蒐集
  - 位置: `user/ws-data-gatherer/SKILL.md`
- ws-decision-maker — 決策評分
  - 位置: `user/ws-decision-maker/SKILL.md`
- yuanbao — 元寶群
  - 位置: `user/yuanbao/SKILL.md`

---

相關連結：[[index|主索引]] | [[system/system-index|System Index]] | [[skills/cron-list|Cron List]] | Daily Stock News | [[finance/finance-index|Finance Index]]
