---
title: "mklab-stock 架構 Q&A（第一批）"
description: "mklab-stock 架構細節的 23 個待決問題，每題 3 方案 + 建議方案（⭐），待用戶思考後回覆"
summary: "mklab-stock 架構未決細節：導航矛盾/Q0、資料管線(Q1-4)、AI層(Q5-7)、前端(Q8-12)、模組資料缺口(Q13-17)、運維(Q18-22)，每題 3 選項含⭐建議"
type: project
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# mklab-stock 架構 Q&A（第一批）

> 狀態：**待用戶思考後回覆**，本頁僅記錄討論選項，尚無定論。
> 對應架構主文：[[mklab-stock-v2-100個功能|mklab-stock 專案架構]]、[[mklab-stock-prompt|實作紀錄與提示詞]]。
> 每題給 3 方案，⭐ 為 Agent 建議方案。

---

## 🔴 Q0. 導航選單（6.0）與 15 模組（6.1~6.15）對不上

| 來源 | 項目 | 狀態 |
| --- | --- | --- |
| 導航 6.0 有 | 📅 Calendar、📝 Report | ❌ 15 模組規格裡沒有這兩個 |
| 15 模組有 | 三 多因子、十一 比較(Compare) | ❌ 導航 6.0 沒列 |
| 導航 6.0 | 🤖 Hermes Agent | ⚠️ 把 Chat/Task/Settings 合成一項，規格是分開三個 |

| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | 導航補齊：加「多因子」「比較」、Chat/Task/Settings 拆開 → 17 項 | 完整但選單擠 |
| B | ⭐ 以 15 模組為準，導航修正為 15 項（Calendar/Report 併入「工具/其他」，Hermes 拆 3 項） | 單一真相源，最清楚 |
| C | 導航維持 14 項，把「多因子/比較」收進 Dashboard 子頁 | 選單簡潔但隱藏功能 |

---

## 一、資料與管線層

### Q1. 08:30 美股資料跨 job 保存
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ Actions **artifact** 暫存（跨 job 保留，不進 repo） | 符合「每日一次 push」，標準做法 |
| B | 08:30 先 commit 到 `data/` 分支 | 違反一次 push 原則 |
| C | 08:30 也 push 一次 main | 最簡但違反決策 |

### Q2. 假日/休市邏輯
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ 內建**交易日曆**（TW+US），非交易日顯示「最後可用日+標註」 | 專業且清楚 |
| B | 照常跑、缺資料留空 | 簡單但用戶困惑 |
| C | 非交易日跳過 workflow（不生成） | 站會停更一天 |

### Q3. 備援源切換觸發
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ 自動：`verify-gate` 失敗 → 自動換下一個源（沿用 quant-trading 邏輯） | 生產級，零人工 |
| B | repo `variables` 手動切 | 可控但需人工 |
| C | 主源掛就整次 skip | 最糟，站停更 |

### Q4. FinMind/OpenBB token 放哪
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ 全走 **repo secrets**（Actions 用），本地 Hermes 用 `.env` | 標準分流 |
| B | 全走 repo secrets（含本地） | 本地每次要同步 |
| C | 明文寫 YAML | 不安全 |

---

## 二、AI / LLM 層

### Q5. 靜態站有無真 AI 內容
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ Actions 跑時呼叫 LLM，把 Summary/Score **烤進 JSON**（靜態站可看 AI 文） | 最符合「AI 分析」需求 |
| B | 靜態站只放量化分數，AI 文全留 Phase 6 Admin | 最省錢但靜態站不「AI」 |
| C | 靜態站放預生成文，Admin 可重新生成 | 折衷但複雜 |

### Q6. 新聞(6.9) 資料源
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **FinMind 新聞 API** + LLM 情緒/摘要 | 結構化、可自動 |
| B | Yahoo 財經 RSS 爬 | 免費但需爬蟲維護 |
| C | 暫不做新聞模組 | 避開最難一塊 |

### Q7. LLM 成本天花板
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **Gemini Flash / GPT-4o-mini**（便宜），月預算設上限 | 成本可控 |
| B | 只用本地模型（Ollama） | 零成本但品質低 |
| C | 高級模型（GPT-4o） | 品質高但貴 |

---

## 三、前端技術

### Q8. 圖表庫
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **ECharts**（echarts-for-react） | 金融 K線/雷達最強 |
| B | Plotly.js | 簡單但重 |
| C | Chart.js | 輕但金融圖弱 |

### Q9. 路由結構
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **React Router + 左側固定 sidebar**（Bloomberg 風） | 專業 |
| B | Tab 切換（單頁不跳轉） | 簡單但難擴 |
| C | 全用 Modal/Overlay | 不適合多模組 |

### Q10. 語系/地區
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **zh-TW 台灣散戶**（NT$ 符號、繁中） | 符合客群 |
| B | 英文國際版 | 公開站但非用戶客群 |
| C | 雙語 i18n | 完整但一倍工作量 |

### Q11. 行動版
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **responsive 降為單欄堆疊**（shadcn 原生） | 標準做法 |
| B | 另做手機專用簡版 | 維護兩套 |
| C | 不支援手機（桌機優先） | 用戶用 iOS，不可行 |

### Q12. 前端測試
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **Vitest（單元）+ 關鍵頁 Playwright（E2E）** | 平衡 |
| B | 只手動測 | 快但易壞 |
| C | 全自動 CI 測 | 重但穩 |

---

## 四、模組級資料缺口

### Q13. ETF 管理費/追蹤誤差/持股 來源
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **TWSE 基金資訊頁 + FinMind** 抓費用/持股 | 權威 |
| B | 只做有資料的欄位，缺的留 null | 快但不完整 |
| C | 手動維護 CSV | 準但累 |

### Q14. 投資組合 IRR/XIRR 輸入
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **CSV 加買入日期欄**，真正算 XIRR；測試資料給範例現金流 | 正確 |
| B | 只算成本 vs 現值，IRR 欄留 null | 簡單但假 |
| C | 不顯示 IRR | 避難 |

### Q15. 多因子 6 因子對齊
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **重寫評分模型對齊 6 因子**（Value/Growth/Momentum/Quality/Volatility/Liquidity） | 符合規格 |
| B | 沿用現有三期模型，映射成 6 因子 | 快但不純 |
| C | 維持三期模型，不強求 6 因子 | 偏離規格 |

### Q16. Heatmap 產業分類
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ DB 已有 `industry` 欄就直接用，缺的補 `fetch_industry` | 複用現有 |
| B | 從 FinMind 抓產業 | 另源 |
| C | 暫不支援產業切換 | 降功能 |

### Q17. 美股資料範圍
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **只抓三大指數**（S&P/Nasdaq/Dow）+ 台股 ADR 前 50 | 輕量夠用 |
| B | 全 S&P500（503 檔） | 全但重 |
| C | 只抓指數不抓個股 | 最輕但無個股頁 |

---

## 五、跨領域 / 運維

### Q18. Data Contract 同步保證
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **CI 檢查**：PR 時比對 Pydantic 與 TS 欄位一致 | 自動防飄移 |
| B | codegen（Pydantic → TS 自動產） | 徹底但綁定工具 |
| C | 手動紀律 | 易漏 |

### Q19. Contract 版本化
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **加 `schema_version` 欄**，舊版前端可降級提示 | 穩健 |
| B | 不版本化，改就同步發 | 簡單但脆 |
| C | 用 git tag 當版本 | 間接 |

### Q20. 雙示警 Email 發送
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **SMTP via repo secret**（Gmail App Password 或 SendGrid） | 真 Email |
| B | 用 GitHub Issues 當「類 Email」通知 | 免憑證但非真郵件 |
| C | 只用 Telegram，Email 暫緩 | 單通道 |

### Q21. Pages 部署模式
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **gh-pages 分支**（構建物隔離，main 乾淨） | 標準最佳實踐 |
| B | `docs/` 資料夾（main 分支） | 簡單但 main 塞滿 |
| C | 外部 CDN 托管 dist | 脫離 Pages |

### Q22. Secrets vs Settings 邊界
| 方案 | 內容 | 評估 |
| --- | --- | --- |
| A | ⭐ **Actions 用 repo secrets；本地 Hermes 用 `.env`；Settings 頁只改本地 .env**（兩套清楚分隔） | 不混淆 |
| B | 全部統一 repo secrets | 本地難用 |
| C | 全部本地 .env | Actions 拿不到 |

---

## 待回覆

用戶思考後回覆每題選項（或全採 ⭐ 建議）。回覆後整理為「最終決策表」寫入 [[mklab-stock-v2-100個功能|架構主文]]。

## 相關節點
- [[mklab-stock-v2-100個功能|mklab-stock 專案架構]]
- [[mklab-stock-prompt|mklab-stock 實作紀錄與提示詞]]
- [[mklab-stock-resource|mklab-stock 資源清單]]
