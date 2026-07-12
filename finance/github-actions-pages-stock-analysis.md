---
title: "GitHub Actions / Pages 在股市分析的應用研究"
description: "研究 ZhuLinsen/daily_stock_analysis 的 GitHub Actions 用法，並規劃我們（Ivan）可套用的架構：Hermes cron 抓資料 → 靜態儀表板 → GitHub Pages 部署"
summary: "①daily_stock_analysis 的 Actions workflow 分析（定時/手動觸發、concurrency、secrets 注入、artifact 上傳、多通道推播）②GitHub Pages 靜態託管在股市的應用邊界（動態圖表/前端分析/回測展示/即時報價）③我們的 Hermes+Pages 架構圖說"
type: resource
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# GitHub Actions / Pages 在股市分析的應用研究

> 研究對象：[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)（LLM 驅動多市場股票智能分析系統，56.5k stars）
> 研究日期：2026-07-13

---

## 一、daily_stock_analysis 的 GitHub Actions 分析

### 它用了哪些 Actions workflow

| Workflow 檔 | 功能 | 核心 Actions 用法 |
| --- | --- | --- |
| `00-daily-analysis.yml` | 每日股票分析主流程 | `schedule: cron`（週一~五 UTC 10:00=北京 18:00）、`workflow_dispatch`（手動+參數）、`concurrency`、`timeout-minutes`、`actions/checkout`、`setup-python`、`upload-artifact` |
| `ci.yml` | PR 門禁 | `dorny/paths-filter`（前端變更才跑 web 測試）、`flake8`、Docker build、`setup-node`+`npm ci`+`lint`+`build` |
| `docker-publish.yml` / `ghcr-dockerhub.yml` | 映像發佈 | Docker Buildx、推送 GHCR/DockerHub |
| `auto-tag.yml` / `create-release.yml` / `desktop-release.yml` | 版本發佈 | 自動打 tag、建 Release、打包桌面版 |
| `network-smoke.yml` | 網路探活 | 定時測資料源連通性 |
| `pr-review.yml` | PR 自動審查 | AI 輔助 review |
| `stale.yml` | Issue 清理 | 自動關閉擱置 Issue |

### 主流程 `00-daily-analysis.yml` 深度拆解

**① 定時觸發（核心）**
```yaml
on:
  schedule:
    - cron: '0 10 * * 1-5'   # 週一到五，UTC 10:00 = 北京 18:00
  workflow_dispatch:          # 手動觸發，帶 mode / force_run 參數
```

**② 防並發與超時**
```yaml
concurrency:
  group: stock-analysis
  cancel-in-progress: false   # 不取消進行中任務，排隊等
timeout-minutes: ${{ fromJSON(vars.ANALYSIS_TIMEOUT_MINUTES || '30') }}
```

**③ 隨機延遲（反爬/分散負載）**
```yaml
- run: sleep $((RANDOM % 60))   # 隨機延遲 0-60 秒
```

**④ Secrets/Variables 注入架構（最值得學的點）**
- 用 `vars.XXX || secrets.XXX` 語法：優先讀 Repository Variables，缺則 fallback 到 Secrets
- AI 模型配置極度模組化：Gemini / OpenAI / DeepSeek / Claude / AIHubMix / Anspire / Hermes / Moonshot / DashScope 各通道獨立注入
- 資料源（Tushare / TickFlow / Longbridge / YFinance）、搜尋引擎（Bocha / Tavily / SerpAPI / SearXNG）、通知（Telegram / Discord / 企業微信 / 飛書 / Slack）全走 Secrets
- 關鍵設計：**`environment: STOCK_LIST`**——把股票清單放在 Environment Variables（而非硬編），隔離配置與代碼

**⑤ 執行與產出**
```yaml
python main.py --market-review   # 或 --no-market-review / --force-run
# 上傳產物
- uses: actions/upload-artifact@v6
  with:
    name: analysis-reports-${{ github.run_number }}
    path: reports/
    retention-days: 30
```

### 關鍵結論
- **它沒用 GitHub Pages**——報告是「上傳 artifact（30天保留）+ 推播到 Telegram/微信/Discord」，**不是**公開網頁
- Actions 的核心價值 = **免費定時跑 Python 分析 + Secret 安全管理 API Key + 產物留存 + 多通道推播**
- 架構成熟度極高（concurrency / timeout / 隨機延遲 / 雙層 vars-secrets / 環境隔離）值得借鑑

---

## 二、我們（Ivan）可以應用的部分

### 可直接借鑑的 Actions 模式
1. **`schedule: cron` 定時觸發** → 我們已有 Hermes cron 做同樣事，但可補 GitHub Actions 作為異地備援（Hermes 掛掉時 Actions 仍跑）
2. **`concurrency` + `timeout-minutes`** → 防止重複抓取/卡死，我們 cron 也可加
3. **`vars || secrets` 雙層注入** → API Key 管理最佳實踐（我們目前 key 在 `/root/.hermes/.env`）
4. **`upload-artifact` 留存報告** → 每日分析結果留痕 30 天，便於回溯
5. **`environment:` 隔離配置** → 股票清單/參數不應硬編進腳本

### GitHub Pages 在股市的應用邊界

> Pages 只能託管**靜態檔案**（HTML/CSS/JS + json/csv）。無後端、無 DB、不能跑 Python。

| 想做的 | Pages 能做？ | 說明 |
| --- | --- | --- |
| 動態網頁 | ⚠️ 部分 | 純前端「動態」（JS 互動、圖表重繪）可以；伺服器端動態不行。需搭配外部 API 或預生成 |
| 分析股票 | ✅ 前端分析 | 用 Plotly/lightweight-charts/TradingView 在前端畫 K 線、技術指標；資料從 CSV/json 或外部 API 抓 |
| 回測股票 | ⚠️ 有限 | 小規模回測可用 JS 在瀏覽器跑；大規模/高頻回測前端會卡，應在 Actions 預跑後把結果頁面化 |
| 股票策略 | ✅ 策略展示 | 策略邏輯寫成 JS 在前端跑；或 Actions 預計算信號 → Pages 顯示信號頁 |
| 即時報價 | ✅ 可嵌 | 用 JS 呼叫免費 API（YFinance/Finnhub/Twse OpenAPI 的 CORS 允許端點）做即時刷新 |

### 對我們最實用的場景
- **每日自動報告網站**：Hermes cron 抓台股資料+算指標 → 生成 `index.html` → 部署 Pages → 打開 `ivanhsia.github.io` 看當天 ETF/大盤儀表板
- **回測結果展示頁**：Actions 跑 `backtest.py` → 產生 `results.json` + 圖 → Pages 渲染成策略績效頁（夏普/最大回撤/權益曲線）
- **選股清單公開頁**：`daily_stock_pick` 結果 → 轉 HTML → Pages 變「每日推薦」公開頁
- **個人量化博客**：現有 Obsidian/Quartz 本身就是 Pages 應用（Quartz = Obsidian 發佈成靜態站）

### 限制提醒
- **台股即時資料 CORS**：Twse/TPEX OpenAPI 多不允許瀏覽器直接跨域抓，前端即時報價常需經 Actions 代理或預生成
- **Pages 不適合敏感資料**：公開可見，持倉/API Key 絕不能放 Pages
- **運算能力**：複雜回測/AI 分析放 Actions 或 Hermes 跑，Pages 只負責「展示結果」

---

## 三、我們可以做的架構圖說

### 架構 A：Hermes 為主 + Pages 展示（推薦，最小改動）

```
┌─────────────────────────────────────────────────────────────┐
│                    每日自動流程（14:00 觸發）                    │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┴───────────────────┐
        ▼                                        ▼
┌──────────────────────┐              ┌──────────────────────┐
│  Hermes cron          │              │  GitHub Actions       │
│  (quant-trading)      │              │  (異地備援，可選)      │
│  - batch1/2/3 抓資料  │              │  - schedule cron       │
│  - daily_stock_pick   │              │  - 同套 Python 腳本     │
│  - backtest.py        │              │  - Secrets 注入 API Key│
└──────────┬───────────┘              └──────────┬───────────┘
           │                                      │
           ▼                                      ▼
┌──────────────────────┐              ┌──────────────────────┐
│  產出 (本地)          │              │  產出 (artifact/網頁) │
│  - tw_stock_all.db    │              │  - reports/*.html     │
│  - reports/*.md       │              │  - dashboard.json      │
└──────────┬───────────┘              └──────────┬───────────┘
           │                                      │
           └──────────────────┬───────────────────┘
                              ▼
              ┌──────────────────────────────┐
              │  生成靜態儀表板 (Python → HTML) │
              │  - Plotly/ECharts K線圖         │
              │  - ETF 列表表格                 │
              │  - 選股推薦卡片                 │
              └──────────────┬───────────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │  git push → GitHub Repo        │
              │  (或 Actions 自動部署)         │
              └──────────────┬───────────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │  GitHub Pages (公開網站)        │
              │  ivanhsia.github.io/quant      │
              │  - 每日儀表板                   │
              │  - 回測績效頁                   │
              │  - 選股清單                     │
              └──────────────┬───────────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                              ▼
      ┌──────────────┐              ┌──────────────┐
      │  Telegram 推播 │              │  Obsidian     │
      │  (現有)        │              │  (Quartz 同步)│
      └──────────────┘              └──────────────┘
```

### 架構 B：純 GitHub Actions 自給自足（零自管伺服器）

```
GitHub Repo
  │
  ├── .github/workflows/
  │     ├── daily-quant.yml    # 每天抓資料+分析+生成 HTML
  │     └── deploy-pages.yml   # 部署到 gh-pages 分支
  │
  ├── scripts/  (quant-trading 核心邏輯)
  ├── templates/  (HTML 範本)
  └── docs/ 或 /public/  (Pages 來源)
        │
        ▼
  GitHub Pages 自動發佈
```

**優點**：完全免伺服器、免 Hermes 也跑得動、API Key 走 Secrets 安全
**缺點**：失去 Hermes 的 AI Agent 彈性（Actions 只能跑預定腳本）

### 我們現有資產對應

| 現有資產 | 在架構中的角色 |
|---------|--------------|
| `quant-trading` (update_all.py) | 資料抓取層 |
| `daily_stock_pick.py` | 選股信號層 |
| `backtest.py` | 回測計算層 |
| `web_server.py:8090` | 可改為「生成靜態 HTML」而非常駐服務 |
| Hermes cron (14:00) | 排程觸發層 |
| Obsidian/Quartz | 知識庫靜態站（已是 Pages 應用） |
| Telegram 推播 | 通知層（保留） |

---

## 四、下一步建議（待用戶確認）

1. **最小可行**：把現有 `web_server.py` 產出的儀表板改成靜態 HTML，Hermes cron 抓完資料後自動生成並 `git push` 到 repo → 開啟 GitHub Pages
2. **中階**：加一個 `deploy-pages.yml` Actions，監聽 repo push 自動部署（不需手動操作）
3. **進階**：Actions 作為 Hermes 的異地備援，雙源定時跑，任一路失敗另一路補

> ⚠️ 敏感資料（持倉明細、API Key）絕不進 Pages；如需展示持倉用匿名化/聚合數據。

## 相關節點
- [[finance-index]]
- [[quant-python-ai-agent]]
- [[dynamic-web-based-financial-analysis-system]]
- [[finance/etf-active-stock/etf-active-stock]]
