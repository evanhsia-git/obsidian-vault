---
title: "quant-dashboard 資源清單"
description: "股市 Dashboard 開發的參考資源——分析平台/Python 框架/Pages 框架/UI 庫/圖表庫/台股資料源/建議架構/學習順序"
summary: "quant-dashboard 專案的資源參考頁：OpenBB/TradingView 等平台、Streamlit/Dash/Reflex 等 Python Dashboard 框架、Quartz/Astro 等靜態框架、shadcn/Tremor UI、Plotly/ECharts 圖表、TWSE/TPEX/FinMind/yfinance 資料源"
type: project
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# quant-dashboard 資源清單

> 參考資源彙整，供 [[finance/quant-dashboard|quant-dashboard 專案架構]] 與 [[finance/quant-dashboard-prompt|實作紀錄]] 選型使用。
> 來源：用戶提供《Python 股票分析 Dashboard 開發指南（Hermes Edition）》。

---

## 一、股票分析平台（參考對象）

| 名稱 | 官方網站 | GitHub | 推薦 |
| --- | --- | --- | --- |
| OpenBB | https://openbb.co | https://github.com/OpenBB-finance/OpenBB | ⭐⭐⭐⭐⭐ |
| TradingView | https://www.tradingview.com | - | ⭐⭐⭐⭐⭐ |
| Finviz | https://finviz.com | - | ⭐⭐⭐⭐⭐ |
| Koyfin | https://www.koyfin.com | - | ⭐⭐⭐⭐ |
| Simply Wall St | https://simplywall.st | - | ⭐⭐⭐⭐ |
| StockAnalysis | https://stockanalysis.com | - | ⭐⭐⭐⭐ |
| MacroTrends | https://www.macrotrends.net | - | ⭐⭐⭐⭐ |
| CompaniesMarketCap | https://companiesmarketcap.com | - | ⭐⭐⭐⭐⭐ |

> 我們的 Dashboard 風格參考 Bloomberg Terminal / TradingView / OpenBB / Koyfin（見 prompt 頁 6.0）。

---

## 二、Python Dashboard 框架

| 框架 | 網址 | GitHub | 用途 |
| --- | --- | --- | --- |
| Streamlit | https://streamlit.io | streamlit/streamlit | 快速 Dashboard |
| Dash | https://dash.plotly.com | plotly/dash | 專業互動圖表 |
| NiceGUI | https://nicegui.io | zauberzeug/nicegui | 全 Python Web UI |
| Panel | https://panel.holoviz.org | holoviz/panel | 科學分析 |
| Bokeh | https://bokeh.org | bokeh/bokeh | 互動圖表 |
| PyGWalker | https://docs.kanaries.net/pygwalker | Kanaries/pygwalker | 視覺分析 |
| Gradio | https://gradio.app | gradio-app/gradio | AI Demo |
| Reflex | https://reflex.dev | reflex-dev/reflex | Python Full Stack |

> ⚠️ **與我們架構的關係**：我們已決定前端用 **React+TS+Tailwind+shadcn/ui**（非 Python 框架）。上表 Streamlit/Dash 等是「全 Python 方案」的替代路線——若未來想零前端學習可用 Reflex（Python Full Stack）重構，但當前架構選 React。

---

## 三、GitHub Pages / 文件框架

- Quartz — https://quartz.jzhao.xyz（我們 Obsidian 已在用）
- Material for MkDocs — https://squidfunk.github.io/mkdocs-material/
- Docusaurus — https://docusaurus.io/showcase
- Astro — https://astro.build/showcase
- Hugo — https://gohugo.io/showcase/
- Jekyll — https://jekyllrb.com/showcase/
- VitePress — https://vitepress.dev
- Nextra — https://nextra.site

> 我們的 quant-dashboard 用 **React (Vite) 直接 build → Pages**，不走上述文件框架（那些是文件/部落格型，非 Dashboard 型）。

---

## 四、Dashboard UI 元件庫

| 庫 | 網址 |
| --- | --- |
| Tremor | https://www.tremor.so |
| shadcn/ui | https://ui.shadcn.com（**我們採用**） |
| Tabler | https://tabler.io |
| Flowbite | https://flowbite.com |
| Mantine | https://mantine.dev |
| Aceternity UI | https://ui.aceternity.com |

---

## 五、圖表庫

- Plotly — https://plotly.com
- Apache ECharts — https://echarts.apache.org
- Chart.js — https://www.chartjs.org
- Highcharts — https://www.highcharts.com

> React 側可用 `echarts-for-react` 或 `plotly.js` 畫 K線/雷達圖/權益曲線。

---

## 六、台股資料來源

| 來源 | 網址 | 角色 |
| --- | --- | --- |
| TWSE | https://www.twse.com.tw | 上市主源 |
| TPEx | https://www.tpex.org.tw | 上櫃/ETF 主源 |
| FinMind | https://finmindtrade.com | 備援 |
| yfinance | https://github.com/ranaroussi/yfinance | 備援 |

> 詳見 [[finance/quant-dashboard|架構頁]] 資料源備援策略。

---

## 七、建議架構（原指南版，供對照）

```
GitHub Actions → Python → FinMind/yfinance → 策略分析 → Plotly
→ Jinja2 → HTML Dashboard → GitHub Pages → Obsidian → Hermes Agent
```

> 此為「Python 全包」路線。我們的架構（見架構頁 v2）改為 **React 前端 + 雙源解耦 + Data Contract**，更適合長期擴充。

---

## 八、學習順序（原指南建議）

1. TradingView
2. OpenBB
3. Streamlit Gallery
4. Dash Gallery
5. NiceGUI
6. Quartz
7. Material for MkDocs
8. shadcn/ui
9. Tremor
10. Astro

---

## 九、TODO（原指南，對照我們專案）

- [ ] 建立股票 Dashboard → 對應我們 Phase 1~5
- [ ] GitHub Actions 自動更新 → 對應雙排程
- [ ] GitHub Pages 發布 → 對應部署
- [ ] Telegram 通知 → 對應雙示警
- [ ] Obsidian 自動同步 → Quartz 已有
- [ ] Hermes Agent 整合 → 對應 Admin Phase 6

---

## 相關節點
- [[finance/quant-dashboard|quant-dashboard 專案架構]]
- [[finance/quant-dashboard-prompt|quant-dashboard 實作紀錄與提示詞]]
- [[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
