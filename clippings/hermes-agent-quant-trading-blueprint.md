---
title: "Hermes Agent 量化交易完整建構方案"
status: active
created: 2026-06-27
updated: 2026-06-27
type: index
description: "Claude 對話紀錄：Hermes Agent 建構台股/美股量化交易平台"
tags:
  - clippings
  - finance
summary: "Hermes Agent 量化交易 — 從零搭建交易系統的 5 Phase 步驟"
---
### Hermes Agent 量化交易建構方案

#### 架構總覽

```
Hermes Agent (Orchestrator)
├── 數據層 (Data Layer)
├── 策略層 (Strategy Layer)
├── 回測層 (Backtest Layer)
├── 執行層 (Execution Layer)
└── 監控層 (Monitor Layer)
```

---
#### 工具棧選擇

| 功能 | 台股/台期 | 美股 | 授權 |
| --- | --- | --- | --- |
| 數據源 | AKShare、FinMind | yfinance、OpenBB | MIT/Free |
| 回測框架 | Backtrader、VectorBT | 同左 | MIT |
| 策略開發 | TA-Lib、pandas-ta | 同左 | Free |
| 執行介面 | 永豐金 API (Shioaji) | IBKR TWS API | Free |
| 期貨數據 | AKShare 台期 | Alpaca | Free tier |
| 排程/佈署 | APScheduler + VPS | 同左 | MIT |
---
#### Phase 1：數據層建構

**安裝核心套件**

```bash
pip install akshare finmind yfinance backtrader vectorbt \
            pandas-ta ta-lib shioaji apscheduler
```

**台股數據抓取 (AKShare)**

```python
# hermes_data_tw.py
import akshare as ak
import pandas as pd

class TWDataFetcher:
    def get_stock_daily(self, symbol: str, start: str, end: str) -> pd.DataFrame:
        """台股日K，symbol如 '2330' """
        df = ak.stock_tw_daily(symbol=symbol, start_date=start, end_date=end)
        df.columns = ['date','open','high','low','close','volume']
        return df.set_index('date')

    def get_futures_daily(self, symbol: str = "TX") -> pd.DataFrame:
        """台指期日K"""
        return ak.futures_tw_daily(symbol=symbol)

    def get_margin_data(self, symbol: str) -> pd.DataFrame:
        """融資融券籌碼"""
        return ak.stock_margin_tw(symbol=symbol)
```

**FinMind 補充財務數據**

```python
# hermes_data_finmind.py
from FinMind.data import DataLoader

class FinMindFetcher:
    def __init__(self, token: str = ""):
        self.dl = DataLoader()
        if token:
            self.dl.login_by_token(api_token=token)

    def get_financial(self, ticker: str):
        return self.dl.taiwan_stock_financial_statement(
            stock_id=ticker, start_date='2020-01-01'
        )

    def get_institutional(self, ticker: str):
        """三大法人買賣"""
        return self.dl.taiwan_stock_institutional_investors(
            stock_id=ticker, start_date='2023-01-01'
        )
```

---
#### Phase 2：策略層建構

**策略基類 + Hermes 訊號整合**

```python
# hermes_strategy_base.py
import pandas as pd
import pandas_ta as ta
from dataclasses import dataclass

@dataclass
class HermesSignal:
    ticker: str
    signal: str        # LONG / SHORT / EXIT / HOLD
    grade: str         # S/A/B/C
    confidence: float  # 0-1
    reason: str
    entry_price: float
    stop_loss: float
    take_profit: float

class StrategyBase:
    def __init__(self, df: pd.DataFrame):
        self.df = df.copy()
        self._add_indicators()

    def _add_indicators(self):
        self.df.ta.macd(append=True)
        self.df.ta.rsi(append=True)
        self.df.ta.bbands(append=True)
        self.df.ta.atr(append=True)
        self.df['ema20'] = self.df.ta.ema(20)
        self.df['ema60'] = self.df.ta.ema(60)

    def generate_signal(self) -> HermesSignal:
        raise NotImplementedError

class MomentumStrategy(StrategyBase):
    """均線 + MACD + RSI 動能策略"""
    def generate_signal(self) -> HermesSignal:
        row = self.df.iloc[-1]
        atr = row.get('ATRr_14', row['close'] * 0.02)

        # 多頭條件
        bull = (
            row['ema20'] > row['ema60'] and
            row['MACD_12_26_9'] > row['MACDs_12_26_9'] and
            30 < row['RSI_14'] < 70
        )
        # 空頭條件
        bear = (
            row['ema20'] < row['ema60'] and
            row['MACD_12_26_9'] < row['MACDs_12_26_9'] and
            row['RSI_14'] > 60
        )

        if bull:
            return HermesSignal(
                ticker="", signal="LONG", grade="B",
                confidence=0.65, reason="EMA+MACD+RSI多頭",
                entry_price=row['close'],
                stop_loss=row['close'] - 2 * atr,
                take_profit=row['close'] + 3 * atr
            )
        elif bear:
            return HermesSignal(
                ticker="", signal="SHORT", grade="B",
                confidence=0.60, reason="EMA+MACD+RSI空頭",
                entry_price=row['close'],
                stop_loss=row['close'] + 2 * atr,
                take_profit=row['close'] - 3 * atr
            )
        return HermesSignal(ticker="", signal="HOLD", grade="C",
                            confidence=0, reason="無明確訊號",
                            entry_price=0, stop_loss=0, take_profit=0)
```

---
#### Phase 3：回測層建構

**Backtrader 回測引擎**

```python
# hermes_backtest.py
import backtrader as bt
import pandas as pd

class HermesBacktestStrategy(bt.Strategy):
    params = dict(
        ema_fast=20, ema_slow=60,
        rsi_period=14, rsi_low=30, rsi_high=70,
        atr_mult_sl=2.0, atr_mult_tp=3.0,
        risk_per_trade=0.02  # 每筆風險2%
    )

    def __init__(self):
        self.ema_f = bt.ind.EMA(period=self.p.ema_fast)
        self.ema_s = bt.ind.EMA(period=self.p.ema_slow)
        self.macd  = bt.ind.MACD()
        self.rsi   = bt.ind.RSI(period=self.p.rsi_period)
        self.atr   = bt.ind.ATR(period=14)

    def next(self):
        if not self.position:
            if (self.ema_f > self.ema_s and
                self.macd.macd > self.macd.signal and
                self.p.rsi_low < self.rsi < self.p.rsi_high):
                # 計算部位大小 (固定風險)
                sl_dist = 2 * self.atr[0]
                size = int((self.broker.cash * self.p.risk_per_trade) / sl_dist)
                self.buy(size=size)
        else:
            if self.ema_f < self.ema_s:
                self.close()

def run_backtest(df: pd.DataFrame, cash: float = 1_000_000) -> dict:
    cerebro = bt.Cerebro()
    cerebro.addstrategy(HermesBacktestStrategy)
    data = bt.feeds.PandasData(dataname=df)
    cerebro.adddata(data)
    cerebro.broker.setcash(cash)
    cerebro.broker.setcommission(commission=0.001425)  # 台股手續費
    cerebro.addanalyzer(bt.analyzers.SharpeRatio, _name='sharpe')
    cerebro.addanalyzer(bt.analyzers.DrawDown, _name='drawdown')
    cerebro.addanalyzer(bt.analyzers.TradeAnalyzer, _name='trades')

    results = cerebro.run()
    strat = results[0]

    return {
        "final_value": cerebro.broker.getvalue(),
        "return_pct": (cerebro.broker.getvalue() - cash) / cash * 100,
        "sharpe": strat.analyzers.sharpe.get_analysis().get('sharperatio'),
        "max_drawdown": strat.analyzers.drawdown.get_analysis().max.drawdown,
        "total_trades": strat.analyzers.trades.get_analysis().total.total
    }
```

**VectorBT 快速向量化回測（參數優化）**

```python
# hermes_vectorbt.py
import vectorbt as vbt
import numpy as np

def vectorbt_param_sweep(close: pd.Series):
    """快速掃描 EMA 參數組合"""
    fast_range = np.arange(10, 30, 5)
    slow_range = np.arange(40, 80, 10)

    fast_ma = vbt.MA.run(close, fast_range, short_name='fast')
    slow_ma = vbt.MA.run(close, slow_range, short_name='slow')

    entries = fast_ma.ma_crossed_above(slow_ma)
    exits   = fast_ma.ma_crossed_below(slow_ma)

    pf = vbt.Portfolio.from_signals(
        close, entries, exits,
        freq='1D', init_cash=1_000_000,
        fees=0.001425
    )
    return pf.stats(['total_return', 'sharpe_ratio', 'max_drawdown'])
```

---
#### Phase 4：執行層（永豐金 Shioaji）

```python
# hermes_executor_tw.py
import shioaji as sj

class HermesExecutor:
    def __init__(self, api_key: str, secret: str, sim: bool = True):
        self.api = sj.Shioaji(simulation=sim)
        self.api.login(api_key, secret)

    def execute_signal(self, signal: HermesSignal):
        if signal.grade not in ['S', 'A']:
            return  # 只執行高等級訊號

        if signal.signal == "LONG":
            order = self.api.Order(
                price=signal.entry_price,
                quantity=1,
                action=sj.constant.Action.Buy,
                price_type=sj.constant.StockPriceType.LMT,
                order_type=sj.constant.OrderType.ROD
            )
            contract = self.api.Contracts.Stocks[signal.ticker]
            trade = self.api.place_order(contract, order)
            return trade

    def set_stop_loss(self, ticker: str, stop_price: float):
        """觸價停損單"""
        pass  # 依 Shioaji 檔案實作
```

---
#### Phase 5：Hermes Agent 整合調度

```python
# hermes_quant_orchestrator.py
import schedule, time
from hermes_data_tw import TWDataFetcher
from hermes_strategy_base import MomentumStrategy
from hermes_backtest import run_backtest
from hermes_executor_tw import HermesExecutor

WATCHLIST = ['2330', '2317', '0050']

def daily_scan():
    fetcher = TWDataFetcher()
    for ticker in WATCHLIST:
        df = fetcher.get_stock_daily(ticker, '2023-01-01', '2025-12-31')
        strat = MomentumStrategy(df)
        signal = strat.generate_signal()
        signal.ticker = ticker

        # 輸出 HSP 格式
        if signal.grade in ['S', 'A', 'B']:
            print(f"[HSP] {ticker} | {signal.signal} | {signal.grade} | {signal.reason}")
            # → 接 Telegram 通知 or 執行器

# 排程：每日收盤後掃描
schedule.every().day.at("14:35").do(daily_scan)
while True:
    schedule.run_pending()
    time.sleep(60)
```

---
#### 部署路線圖

```
Week 1  數據層驗證 (AKShare/FinMind 接通，歷史數據完整)
Week 2  策略開發 + 單一標的回測 (Backtrader)
Week 3  VectorBT 參數優化 + 多標的回測
Week 4  Shioaji 模擬帳戶接線 (simulation=True)
Week 5+ 小資金實盤驗證 → Hermes 自動調度上線
```

#### 風險控制硬規則（寫入 SOUL.md）

```yaml
max_position_per_stock: 10%   # 單股不超過總資金10%
max_daily_loss: 3%            # 日虧損超過3%停止交易
signal_grade_floor: B         # C級訊號不執行
backtest_min_sharpe: 0.8      # Sharpe < 0.8策略不上線
backtest_min_trades: 30       # 樣本數不足不上線
[HOLD] required for: S-grade  # S級訊號強制人工確認
```

**Claude**
hermes agent 量化交易架構