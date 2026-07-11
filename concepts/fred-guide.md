---
title: "FRED (Federal Reserve Economic Data) 指南"
description: "FRED (Federal Reserve Economic Data) 指南 — 概念說明頁面"
summary: "FRED (Federal Reserve Economic Data) 指南"

type: concept
status: active

tags: [finance]
created: 2026-06-16
updated: 2026-06-16



---
# FRED (Federal Reserve Economic Data) 指南

FRED（Federal Reserve Economic Data）雖然是由聖路易斯聯邦儲備銀行維護的「經濟」資料庫，但它與全球各大交易所、指數公司（如 S&P Dow Jones、NASDAQ、CBOE）有長期的官方資料合作。

## 一、 概述

### ⚠️ 核心限制與使用心法

- **非即時性**：FRED 不提供盤中秒級或分級的即時串流報價（Tick Data），其數據多為每日收盤價（EOD）、週均值或月均值。
- **歷史縱深長**：其核心優勢在於擁有長達數十年、未經刪減的連續歷史數據，非常適合用於中長線趨勢分析、量化策略回測（Backtesting）以及跨資產總經研究。

## 二、 核心股市行情與衍生指標清單

在 FRED 系統中，每一種數據都由一個獨一無二的 **Series ID（代號）** 標記。以下為美股分析必備的五大核心模組：

### 1. 美股三大權威大盤指數
反映美國股市整體走勢的最直接行情：
- **SP500 (標普 500 指數)**：覆蓋美國 500 家大型上市公司，代表美股大盤核心。
- **DJIA (道瓊工業平均指數)**：歷史最悠久的 30 檔藍籌股行情。
- **NASDAQCOM (納斯達克綜合指數)**：科技股、成長股與生技產業的風向球。
- **WILL5000IND (威爾遜 5000 全市場指數)**：涵蓋全美幾乎所有公開上市股票。其市值常被總經分析師拿來除以美國 GDP，用以計算著名的「巴菲特指標」。

### 2. 市場情緒與恐慌指標
- **VIXCLS (CBOE 波動率指數 / 恐慌指數)**：利用標普 500 指數選擇權隱含波動率計算而得。當 VIX 飆高（通常大於 30），代表市場出現恐慌性拋售，往往也是股市尋找中長線底部的訊號。

### 3. 金融壓力與系統性風險（股市領先/同步指標）
這是 FRED 最無可替代的資產，能比單純的股價圖更早反映金融體系的「失血」或「流動性危機」：
- **STLFSI4 (聖路易斯聯儲金融壓力指數)**：由 18 個日度和週度金融變數（包含利差、波動度等）加權計算。
    - **大於 0**：代表金融市場壓力高於歷史平均值（股市通常面臨修正）。
    - **小於 0**：代表金融市場環境寬鬆（對股市上漲有利）。
- **NFCI (芝加哥聯儲全國金融狀況指數)**：涵蓋 105 個貨幣、股票和債券市場指標，用於評估美國整體金融體系的健康度。

### 4. 跨資產連動因子（股市定價的「錨」）
股票不可能脫離債券與外匯市場單獨上漲，這三個指標在量化模型中權重極高：
- **DGS10 (美國 10 年期公債殖利率)**：無風險利率的基準。當其快速飆升時，會嚴重壓低科技股與高估值股票的折現值。
- **T10Y2Y (10年期減2年期公債利差)**：即著名的「殖利率倒掛」指標。當此數值跌破 0，歷史上通常預示著 1 到 2 年內經濟將步入衰退，美股容易出現大熊市。
- **DTWEXBGS (美元指數 - 貿易加權)**：美元過強通常會壓制跨國美企的海外營收，且會引發新興市場資金外流，對全球股市具有負面效應。

## 三、 Python 自動化抓取與清理實戰

若要在 Python 中完整提取並整合上述行情，我們需要解決兩個實務問題：
1. **自動環境變數讀取**（安全儲存 API Key）。
2. **假日缺失值填補**（股市休市日、經濟數據發布日不同步的問題）。

### 1. 事前準備
請確保您的開發環境已安裝以下套件：
```bash
pip install fredapi pandas matplotlib python-dotenv
```

### 2. 建立環境變數檔案
在您的專案根目錄下建立一個名為 `.env` 的檔案，內容如下：
```text
FRED_API_KEY=your_actual_32_character_api_key_here
```

### 3. 完整 Python 程式碼
這段程式碼將自動載入金鑰，同時抓取「大盤行情」、「恐慌指數」與「十年期美債」，並自動對齊日期、填補休市空值。

```python
import os
import pandas as pd
import matplotlib.pyplot as plt
from dotenv import load_dotenv
from fredapi import Fred

# 1. 讀取 .env 檔案中的 API Key
load_dotenv()
API_KEY = os.getenv("FRED_API_KEY")

if not API_KEY:
    raise ValueError("找不到 FRED_API_KEY，請確認 .env 檔案設定是否正確。")

# 2. 初始化 FRED 實例
fred = Fred(api_key=API_KEY)

# 3. 定義欲抓取的股市與宏觀指標
indicators = {
    'S&P 500': 'SP500',
    'VIX': 'VIXCLS',
    'US_10Y_Yield': 'DGS10'
}

data_dict = {}

# 4. 循環抓取數據
print("正在從 FRED 獲取歷史行情資料...")
for name, series_id in indicators.items():
    try:
        data_dict[name] = fred.get_series(series_id)
        print(f"✅ {name} ({series_id}) 抓取成功！")
    except Exception as e:
        print(f"❌ {name} 抓取失敗: {e}")

# 5. 資料清理與對齊
# 將多個 Time Series 合併為一個 DataFrame
df = pd.DataFrame(data_dict)

# 由於各指標更新頻率或休市日不同，使用「前向填補(ffill)」處理假日缺失值
df = df.ffill()

# 6. 輸出最近 10 個交易日的行情報告
print("\n=== 最近 10 個交易日行情數據 ===")
print(df.tail(10))

# 7. (選填) 儲存成 CSV 報告以便在 Excel 中查看
df.to_csv("fred_stock_report.csv")
print("\n報告已成功儲存為 fred_stock_report.csv")
```

## 四、 報告總結與下一步建議

透過 FRED API，您可以輕鬆架構出一個「總經 + 股市行情」的自動化監控儀表板。這份資料最核心的價值不在於短線當沖，而在於「風控」。當您看到 STLFSI4（金融壓力）突然轉為正數，或者 T10Y2Y（利差）從倒掛開始劇烈「陡峭化」回升時，這份數據就能提早為您的股票部位發出撤退或減碼的警訊。

---
## 相關節點
- [[us-cpi-analysis]]
- us-gdp-analysis