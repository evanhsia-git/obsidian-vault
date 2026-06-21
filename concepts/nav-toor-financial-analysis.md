---
title: Nav Toor   Financial Analysis Prompts
summary: Nav Toor   Financial Analysis Prompts：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: [maintenance]
---

## 相關頁面
- [[concepts/financial-preferences|金融偏好]]
- [[concepts/hermes-configuration|Hermes 配置]]


# Nav Toor: 投資銀行級財務分析 Prompt 彙整

這套 Prompt 旨在將 AI 轉化為頂尖投行 (如 Goldman Sachs, Morgan Stanley) 級別的分析工具，將簡單的摘要提升至私募股權 (Private Equity) 級別的定量分析。

---

## 1. 運營模式與單位經濟效益 (Operating Model & Unit Economics)

### 💡 分析重點
- **極高標準角色 (Persona)**：設定為 「General Atlantic 的成長股投資人」，驅使 AI 關注「可擴展性 (Scalability)」與「風險控制」。
- **自下而上構建 (Bottom-up)**：要求展示營收構建 (Revenue Build) 過程，而非直接給予結果。
- **核心指標分析**：強制分析 CAC (獲客成本) 與 LTV (生命週期價值) 的關係。
- **動態風險管理**：引入情景規劃 (Upside, Base, Downside) 與燃燒率 (Burn Rate) 計算。
- **嚴格時間維度**：第一年每月預測 → 第二、三年每季預測。

### 📝 Prompt 模板
> 您是 General Atlantic 的成長股投資人。我需要了解 [公司名稱] 的詳細經營模式。
> 
> 請提供：
> - **營收預測**：透過客戶、產品或地區等維度進行自下而上的預測
> - **單位經濟指標**：獲客成本、客戶生命週期價值、回報期、每單位的毛利率
> - **群組分析**：不同年齡層的客戶隨時間的表現如何
> - **主要影響因素**：是什麼導致了收入和成本的變動？
> - **情景規劃**：上漲、持平、下跌三種情況的假設
> - **燃燒率**：每月的現金消耗量及資金維持期計算
> - **盈虧平衡分析**：當公司的現金流為正數時
> - **擴張假設**：隨著業務成長，單位經濟效益如何提升
> 
> 採用運營模式來呈現數據，第一年為每月的預測數據，第二至三年則為每季的預測數據。
> 
> 公司：[說明商業模式、現有指標及成長率]

---

## 2. 核心財務分析 Prompt (四大關鍵任務)

### 2.1 現金流量折現模型 (DCF Model)
- **目的**：估算公司目前價值。
- **角色**：高盛高級分析師 (Senior Analyst)。
- **分析核心**：未來 5 年自由現金流 → WACC 折現 → 終端價值 → 敏感度分析。
- **產出**：牛市 (Bull)、基準 (Base)、熊市 (Bear) 的估值範圍。

### 2.2 三表財務模型 (Three-Statement Model)
- **目的**：分析財務互連性。
- **角色**：摩根士丹利副總裁 (Vice President)。
- **分析核心**：損益表 → 資產負債表 → 現金流量表 的聯動邏輯，包含營運資本與債務還款計畫。
- **產出**：完整 5 年財務預測模型及 Excel 格式邏輯說明。

### 2.3 可比公司分析 (Comparable Company Analysis / Comps)
- **目的**：透過同業定價參照公司價值。
- **角色**：花旗銀行股票研究分析師 (Equity Research Analyst)。
- **分析核心**：篩選 10-15 家對標公司 → 比較 EV/EBITDA、EV/Revenue、P/E 倍數 → 計算溢價/折價原因。
- **產出**：包含分位數 (25th, Median, 75th) 的估值對比表。

### 2.4 IPO 估值與定價分析 (IPO Valuation)
- **目的**：確定上市定價策略。
- **角色**：巴克萊資本市場銀行家 (Capital Markets Banker)。
- **分析核心**：Pre-Money/Post-Money 估值 → 稀釋分析 → 流通盤分析 → 定價區間。
- **產出**：IPO 定價備忘錄 (Pricing Memo) 及建議價格區間。

---

## 🚀 核心邏輯總結
1. **角色設定** → 定義專業深度 (專業角色 = 專業框架)。
2. **推演過程** → 強制自下而上 (Bottom-up) 構建，避免 AI 隨意捏造數字。
3. **動態變數** → 使用情景規劃與壓力測試 (Scenario Planning)。
4. **交付標準** → 模仿頂尖投行 (IB) 的報告格式與時間粒度。

相關頁面：[[environment-keys|Environment Keys]]

相關頁面：[[news-push-log]]


## 相關節點
- [[index]]
