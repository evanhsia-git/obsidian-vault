---
title: "ta-lib 技術指標完整指南"
description: "ta-lib 技術指標列表、分類、用途與 Python 範例。涵蓋 Overlap、Momentum、Volume、Volatility、Cycle、Pattern、Statistic 七大類"
summary: "ta-lib 200+ 技術指標完整分類：均線、動量、成交量、波動率、週期、形態識別、統計"
type: resource
status: active
tags: [ta-lib, finance, stock-analysis, python, technical-analysis]
created: 2026-06-29
updated: 2026-06-29
---

# ta-lib 技術指標完整指南

> ta-lib 是 C 語言編寫的技術分析庫，Python wrapper 提供 200+ 指標。
> 安裝：`pip install ta-lib`（需先裝 C 語言底層 libta-lib）

---

## 1. Overlap Studies（均線重疊指標）

計算價格均線，用於判斷趨勢方向與支撐/阻力位。

| 指標 | 全名 | 用途 |
|------|------|------|
| `SMA` | Simple Moving Average | 簡單移動平均線，最基礎的趨勢指標 |
| `EMA` | Exponential Moving Average | 指數移動平均，對近期價格更敏感 |
| `WMA` | Weighted Moving Average | 加權移動平均，線性加權近期數據 |
| `DEMA` | Double Exponential MA | 雙倍指數移動平均，減少 SMA 滯後 |
| `TEMA` | Triple Exponential MA | 三倍指數移動平均，更平滑且滯後更低 |
| `T3` | Triple Exponential MA (T3) | T3 自適應均線，可調平滑係數 |
| `KAMA` | Kaufman Adaptive MA | 自適應均線，根據市場波動自動調整速度 |
| `MAMA` | MESA Adaptive MA | MESA 自適應均線，基於 Hilbert Transform |
| `TRIMA` | Triangular MA | 三角形移動平均（SMA 的 SMA） |
| `MA` | Moving Average | 通用移動平均（可選擇類型） |
| `MAVP` | Moving Average with Variable Period | 可變週期移動平均 |
| `MIDPOINT` | MidPoint over period | 週期內最高最低的中點 |
| `MIDPRICE` | Midpoint Price over period | 週期內最高最低價的平均 |
| `BBANDS` | Bollinger Bands | 布林通道：中線(SMA) + 上下 2σ 軌道 |
| `SAR` | Parabolic SAR | 拋物線 SAR，用於停損與反轉判斷 |
| `SAREXT` | Parabolic SAR Extended | 擴展版 SAR，支援更多參數 |
| `HT_TRENDLINE` | Hilbert Transform Trendline | Hilbert 轉換即時趨勢線 |

---

## 2. Momentum Indicators（動量指標）

衡量價格變動的速度與力度，用於判斷超買超賣與趨勢反轉。

| 指標 | 全名 | 用途 |
|------|------|------|
| `RSI` | Relative Strength Index | 相對強弱指數（0-100），>70 超買、<30 超賣 |
| `MACD` | Moving Average Convergence/Divergence | MACD = EMA12 - EMA26，Signal = EMA9(MACD) |
| `MACDEXT` | MACD with controllable MA type | 可自選 MACD 的 MA 類型 |
| `MACDFIX` | MACD Fix 12/26 | 固定 12/26 參數的 MACD |
| `MOM` | Momentum | 動量 = close[t] - close[t-n] |
| `ROC` | Rate of Change | 變動率 = (close[t] - close[t-n]) / close[t-n] × 100 |
| `ROCP` | Rate of Change Percentage | 變動率百分比（小數形式） |
| `ROCR` | Rate of Change Ratio | 變動比率 = close[t] / close[t-n] |
| `ROCR100` | Rate of Change Ratio 100 | 變動比率 × 100（如 RSI 格式） |
| `PPO` | Percentage Price Oscillator | 價格震盪百分比 = (EMA_short - EMA_long) / EMA_long × 100 |
| `APO` | Absolute Price Oscillator | 絕對價格震盪 = EMA_short - EMA_long |
| `TRIX` | 1-day Rate-Of-Change of Triple Smooth EMA | 三重平滑 EMA 的變動率，過濾短期噪音 |
| `CCI` | Commodity Channel Index | 商品通道指數，判斷超買超賣與趨勢強度 |
| `CMO` | Chande Momentum Oscillator | Chande 動量震盪（-100 到 +100） |
| `DX` | Directional Movement Index | 方向性運動指數，衡量趨勢強度 |
| `ADX` | Average Directional Movement Index | ADX 的移動平均，趨勢強度指標 |
| `ADXR` | Average Directional Movement Index Rating | ADX 的平滑版 |
| `MINUS_DI` | Minus Directional Indicator | 負方向指標（-DI） |
| `PLUS_DI` | Plus Directional Indicator | 正方向指標（+DI） |
| `MINUS_DM` | Minus Directional Movement | 負方向運動 |
| `PLUS_DM` | Plus Directional Movement | 正方向運動 |
| `AROON` | Aroon | 顯示趨勢強度與方向（Aroon Up/Down） |
| `AROONOSC` | Aroon Oscillator | Aroon 震盪 = AroonUp - AroonDown |
| `BOP` | Balance Of Power | 力量平衡 = (close - open) / (high - low) |
| `MFI` | Money Flow Index | 資金流量指數（含成交量，類似 RSI 但加權成交量） |
| `STOCH` | Stochastic | 隨機指標 %K/%D，判斷超買超賣 |
| `STOCHF` | Stochastic Fast | 快速隨機指標（較少平滑） |
| `STOCHRSI` | Stochastic RSI | RSI 的隨機振盪，更敏感 |
| `ULTOSC` | Ultimate Oscillator | 終極震盪（加權三個不同週期的動量） |
| `WILLR` | Williams %R | 威廉指標（-100 到 0），超買超賣 |

---

## 3. Volume Indicators（成交量指標）

結合價格與成交量，確認趨勢可靠性。

| 指標 | 全名 | 用途 |
|------|------|------|
| `OBV` | On Balance Volume | 能量潮，成交量累加判斷趨勢方向 |
| `AD` | Chaikin A/D Line | 累積/派發線，量價背離判斷 |
| `ADOSC` | Chaikin A/D Oscillator | A/D 線的 MACD 式震盪 |
| `FI` | Force Index | 力量指數 = (close[t] - close[t-1]) × volume |

---

## 4. Volatility Indicators（波動率指標）

衡量價格波動幅度，用於風險評估與停損。

| 指標 | 全名 | 用途 |
|------|------|------|
| `ATR` | Average True Range | 平均真實波幅，最常用的波動率指標 |
| `NATR` | Normalized ATR | 標準化 ATR = ATR / close × 100，跨價格比較 |
| `TRANGE` | True Range | 真實波幅 = max(high-low, \|high-prev_close\|, \|low-prev_close\|) |

---

## 5. Cycle Indicators（週期指標）

識別價格的週期性波動。

| 指標 | 全名 | 用途 |
|------|------|------|
| `HT_DCPERIOD` | Hilbert Transform Dominant Cycle Period | 主週期長度 |
| `HT_DCPHASE` | Hilbert Transform Dominant Cycle Phase | 週期的當前相位 |
| `HT_PHASOR` | Hilbert Transform Phasor Components | Hilbert 相位分量（In-Phase / Quadrature） |
| `HT_SINE` | Hilbert Transform SineWave | Hilbert 正弦波（Sine / LeadSine） |
| `HT_TRENDMODE` | Hilbert Transform Trend vs Cycle Mode | 趨勢模式 vs 週期模式判斷 |

---

## 6. Pattern Recognition（K 線形態識別）

自動識別常見的 K 線反轉與持續形態。

| 指標 | 全名 | 用途 |
|------|------|------|
| `CDL2CROWS` | Two Crows | 兩隻烏鴉（看空反轉） |
| `CDL3BLACKCROWS` | Three Black Crows | 三隻黑烏鴉（看空反轉） |
| `CDL3INSIDE` | Three Inside Up/Down | 三內上升/下降（反轉） |
| `CDL3LINESTRIKE` | Three-Line Strike | 三線打擊 |
| `CDL3OUTSIDE` | Three Outside Up/Down | 三外上升/下降 |
| `CDL3STARSINSOUTH` | Three Stars In The South | 南方三星（看多） |
| `CDL3WHITESOLDIERS` | Three White Soldiers | 三白兵（看多） |
| `CDLABANDONEDBABY` | Abandoned Baby | 棄嬰（反轉） |
| `CDLADVANCEBLOCK` | Advance Block | 推進塊（看空） |
| `CDLBELTHOLD` | Belt-hold | 腰帶抱線 |
| `CDLBREAKAWAY` | Breakaway | 突破 |
| `CDLCLOSINGMARUBOZU` | Closing Marubozu | 收盤一字線 |
| `CDLCONCEALBABYSWALL` | Concealing Baby Swallow | 吞沒棄嬰 |
| `CDLCOUNTERATTACK` | Counterattack | 反擊 |
| `CDLDARKCLOUDCOVER` | Dark Cloud Cover | 烏雲蓋頂（看空） |
| `CDLDOJI` | Doji | 十字線（猶豫） |
| `CDLDOJISTAR` | Doji Star | 十字星（反轉） |
| `CDLDRAGONFLYDOJI` | Dragonfly Doji | 蜻蜓十字（看多反轉） |
| `CDLENGULFING` | Engulfing | 吞沒形態 |
| `CDLEVENINGDOJISTAR` | Evening Doji Star | 黃昏十字星（看空） |
| `CDLEVENINGSTAR` | Evening Star | 黃昏星（看空反轉） |
| `CDLGAPSIDESIDEWHITE` | Gap Side-by-Side White | 並列白線（缺口） |
| `CDLGRAVESTONEDOJI` | Gravestone Doji | 墓碑十字（看空） |
| `CDLHAMMER` | Hammer | 錘子線（看多反轉） |
| `CDLHANGINGMAN` | Hanging Man | 吊人線（看空反轉） |
| `CDLHARAMI` | Harami | 母子線 |
| `CDLHARAMICROSS` | Harami Cross | 十字母子 |
| `CDLHIGHWAVE` | High-Wave | 高波線 |
| `CDLHIKKAKE` | Hikkake | 陷阱形態 |
| `CDLHIKKAKEMOD` | Modified Hikkake | 改良陷阱 |
| `CDLHOMINGPIGEON` | Homing Pigeon | 歸巢鴿 |
| `CDLIDENTICAL3CROWS` | Identical Three Crows | 三同黑烏鴉 |
| `CDLINNECK` | In-Neck | 頸線 |
| `CDLINVERTEDHAMMER` | Inverted Hammer | 倒錘（看多） |
| `CDLKICKING` | Kicking | 踢腿線 |
| `CDLKICKINGBYLENGTH` | Kicking By Length | 長踢腿 |
| `CDLLADDERBOTTOM` | Ladder Bottom | 梯底（看多） |
| `CDLLONGLEGGEDDOJI` | Long Legged Doji | 長腿十字 |
| `CDLLONGLINE` | Long Line | 長線 |
| `CDLMARUBOZU` | Marubozu | 一字線（強勢） |
| `CDLMATCHINGLOW` | Matching Low | 同低 |
| `CDLMATHOLD` | Mat Hold | 墊高 |
| `CDLMORNINGDOJISTAR` | Morning Doji Star | 早晨十字星（看多） |
| `CDLMORNINGSTAR` | Morning Star | 早晨星（看多反轉） |
| `CDLONNECK` | On-Neck | 頸線 |
| `CDLPIERCING` | Piercing | 穿刺線（看多） |
| `CDLRICKSHAWMAN` | Rickshaw Man | 黃包車夫 |
| `CDLRISEFALL3METHODS` | Rising/Falling Three Methods | 上升/下降三法 |
| `CDLSEPARATINGLINES` | Separating Lines | 分離線 |
| `CDLSHOOTINGSTAR` | Shooting Star | 流星線（看空） |
| `CDLSHORTLINE` | Short Line | 短線 |
| `CDLSPINNINGTOP` | Spinning Top | 紡錘線 |
| `CDLSTALLEDPATTERN` | Stalled Pattern | 停滯形態 |
| `CDLSTICKSANDWICH` | Stick Sandwich | 三明治 |
| `CDLTAKURI` | Takuri | 探水竿 |
| `CDLTASUKIGAP` | Tasuki Gap | 祐光缺口 |
| `CDLTHRUSTING` | Thrusting | 推力 |
| `CDLTRISTAR` | Tristar | 三星 |
| `CDLUNIQUE3RIVER` | Unique 3 River | 三河 |
| `CDLUPSIDEGAP2CROWS` | Upside Gap Two Crows | 上升缺口兩烏鴉 |
| `CDLXSIDEGAP3METHODS` | Upside/Downside Gap Three Methods | 上升/下降缺口三法 |

---

## 7. Statistic Functions（統計指標）

統計分析指標，用於量化價格分佈與相關性。

| 指標 | 全名 | 用途 |
|------|------|------|
| `BETA` | Beta | 衡量個股與市場的相關性/敏感度 |
| `CORREL` | Pearson Correlation | 皮爾森相關係數 |
| `LINEARREG` | Linear Regression | 線性回歸 |
| `LINEARREG_ANGLE` | Linear Regression Angle | 回歸線角度 |
| `LINEARREG_INTERCEPT` | Linear Regression Intercept | 回歸截距 |
| `LINEARREG_SLOPE` | Linear Regression Slope | 回歸斜率 |
| `STDDEV` | Standard Deviation | 標準差 |
| `TSF` | Time Series Forecast | 時間序列預測 |
| `VAR` | Variance | 變異數 |

---

## 8. Price Transform（價格轉換）

| 指標 | 全名 | 用途 |
|------|------|------|
| `AVGPRICE` | Average Price | 均價 = (open+high+low+close)/4 |
| `MEDPRICE` | Median Price | 中位價 = (high+low)/2 |
| `TYPPRICE` | Typical Price | 典型價 = (high+low+close)/3 |
| `WCLPRICE` | Weighted Close Price | 加權收盤 = (high+low+close+close)/4 |

---

## Python 快速範例

```python
import numpy as np
import talib

close = np.random.random(100)
high = np.random.random(100)
low = np.random.random(100)
volume = np.random.random(100)

# 均線
sma20 = talib.SMA(close, timeperiod=20)
ema12 = talib.EMA(close, timeperiod=12)

# 動量
rsi = talib.RSI(close, timeperiod=14)
macd, signal, hist = talib.MACD(close, fastperiod=12, slowperiod=26, signalperiod=9)

# 波動率
atr = talib.ATR(high, low, close, timeperiod=14)

# 成交量
obv = talib.OBV(close, volume)

# 形態識別
doji = talib.CDLDOJI(np.zeros(100), high, low, close)
hammer = talib.CDLHAMMER(np.zeros(100), high, low, close)

# 統計
beta = talib.BETA(close, close, timeperiod=5)
```

---

## 與我們系統的關聯

| 系統 | 使用方式 |
|------|----------|
| `quant-trading` skill | 目前用自訂 scoring 因子（PE/PB/殖利率/ROE），尚未用 ta-lib |
| `daily_stock_pick.py` | 可加入 ta-lib 指標作為選股因子 |
| `backtest.py` | 可用 ta-lib 指標做回測策略 |

**未來擴充建議**：
1. 安裝 ta-lib C 底層 + Python wrapper
2. 在 `daily_stock_pick.py` 加入 RSI、MACD、布林通道等因子
3. 用 ATR 做動態停損
4. 用 MFI 做成交量加權選股
