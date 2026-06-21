---
title: 股票組合回測分析 (AAPL, MSFT, AMZN, NVDA)
summary: 股票組合回測分析 (AAPL, MSFT, AMZN, NVDA)：策略說明
created: 2026-06-03
updated: 2026-06-03
type: query
tags: [quantitative-trading, backtest, stock-analysis, aapl, msft, amzn, nvda]
sources: [https://www.kimi.com/replay/19b46548-f4d2-80ad-8000-0000db1d5f50]
---

# 股票組合回測分析

本報告針對由 AAPL、MSFT、AMZN 和 NVDA 組成的等權重投資組合進行了回測。

## 策略說明
- **資產組合**：等權重 (AAPL, MSFT, AMZN, NVDA)
- **回測期間**：過去三年 (每日數據)
- **策略邏輯**：趨勢跟蹤 (持有條件：價格 > 200 日均線)
- **調整頻率**：每月再平衡
- **交易成本**：0.1%

## 績效對比表

| 指標 | 趨勢跟蹤策略 | 買入持有策略 | 優勢方 |
| :--- | :--- | :--- | :--- |
| 總收益率 | 101.4% | 120.4% | 買入持有 |
| 年化複合增長率 | 26.3% | 30.2% | 買入持有 |
| 夏普比率 | 1.194 | 1.101 | 趨勢跟蹤 |
| 最大回撤 | -20.8% | -28.9% | 趨勢跟蹤 |
| 卡爾瑪比率 | 1.264 | 1.043 | 趨勢跟蹤 |
| 年化波動率 | 19.6% | 25.1% | 趨勢跟蹤 |

## 關鍵發現
1. **風險調整收益**：趨勢跟蹤策略在夏普比率與卡爾瑪比率上均優於買入持有策略。
2. **波動率控制**：趨勢跟蹤有效降低了整體組合的波動率 (19.6% vs 25.1%) 與最大回撤。
3. **交易频率**：3 年換手率為 74 次，月均約 2 次。

## 報告連結
- [線上詳細報告](https://www.kimi.com/replay/19b46548-f4d2-80ad-8000-0000db1d5f50)

## 相關概念
- [[entities/finlab|FinLab]]
- [[entities/trading-agents|TradingAgents]]
