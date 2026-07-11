---
title: "Dynamic Web-Based Financial Analysis System"
description: "寫出一個動態網頁搭配建立的資料庫，只在網路上做執行分析，回測，需要那些工具，那些網站，那些架構，我要如何請agent做建構"
summary: "動態網頁金融分析系統"
type: schema
status: active
tags:
  - finance
  - web
  - pkm
created: 2026-06-28
updated: 2026-06-28
---


!pasted image 20260628212010.png

### 技術棧選擇（全免費開源）

**前端**：React + Vite（或 Next.js）、Chart.js / Recharts（K線圖）、TailwindCSS

**後端 API**：FastAPI（Python）— 非同步、自動生成 API 檔案

**即時通訊**：WebSocket（FastAPI 內建）— 推送訊號、回測進度

**任務佇列**：Celery + Redis — 背景執行耗時的回測任務

**資料庫**：TimescaleDB（PostgreSQL 延伸，專為時序 K 棒設計）+ Redis 快取

**佈署**：Docker Compose 一鍵啟動所有服務

---

### 五個核心建構步驟

#### Step 1：資料庫設計（TimescaleDB）

sql

```sql
-- K棒時序表（自動分片優化）
CREATE TABLE ohlcv (
  time        TIMESTAMPTZ NOT NULL,
  symbol      TEXT NOT NULL,
  market      TEXT NOT NULL,  -- 'TW' | 'US' | 'TWF'
  open        NUMERIC,
  high        NUMERIC,
  low         NUMERIC,
  close       NUMERIC,
  volume      BIGINT
);
SELECT create_hypertable('ohlcv', 'time');
CREATE INDEX ON ohlcv (symbol, time DESC);

-- HSP訊號記錄表
CREATE TABLE hsp_signals (
  id          SERIAL PRIMARY KEY,
  packet_id   TEXT UNIQUE,
  created_at  TIMESTAMPTZ DEFAULT NOW(),
  ticker      TEXT,
  grade       TEXT,       -- S/A/B/C
  signal_type TEXT,
  confidence  NUMERIC,
  payload     JSONB
);

-- 回測結果表
CREATE TABLE backtest_results (
  id              SERIAL PRIMARY KEY,
  strategy_name   TEXT,
  params          JSONB,
  run_at          TIMESTAMPTZ DEFAULT NOW(),
  sharpe_ratio    NUMERIC,
  max_drawdown    NUMERIC,
  total_return    NUMERIC,
  total_trades    INT,
  equity_curve    JSONB   -- 存逐日淨值
);
```

#### Step 2：FastAPI 後端骨架

python

```python
# main.py
from fastapi import FastAPI, WebSocket
from fastapi.middleware.cors import CORSMiddleware
from celery_app import run_backtest_task
import asyncio, json

app = FastAPI(title="Hermes Quant API")
app.add_middleware(CORSMiddleware, allow_origins=["*"])

# 觸發回測（非同步，丟給 Celery）
@app.post("/api/backtest/run")
async def trigger_backtest(payload: dict):
    task = run_backtest_task.delay(payload)
    return {"task_id": task.id, "status": "queued"}

# 查詢回測結果
@app.get("/api/backtest/{task_id}")
async def get_backtest(task_id: str):
    from celery_app import app as celery
    result = celery.AsyncResult(task_id)
    return {"status": result.status, "result": result.result}

# WebSocket：Hermes 即時訊號推送
@app.websocket("/ws/signals")
async def signal_stream(ws: WebSocket):
    await ws.accept()
    # 連接 Redis pub/sub，有新 HSP 就推送給前端
    import aioredis
    redis = await aioredis.from_url("redis://localhost")
    pubsub = redis.pubsub()
    await pubsub.subscribe("hermes_signals")
    async for message in pubsub.listen():
        if message["type"] == "message":
            await ws.send_text(message["data"])
```

#### Step 3：Celery 回測工作

python

```python
# celery_app.py
from celery import Celery
import backtrader as bt, json

app = Celery("hermes", broker="redis://localhost:6379/0",
             backend="redis://localhost:6379/1")

@app.task(bind=True)
def run_backtest_task(self, payload: dict):
    """背景執行回測，完成後存 DB + 推送 WebSocket"""
    try:
        self.update_state(state="RUNNING", meta={"progress": 10})
        
        # 從 DB 撈資料
        df = load_ohlcv_from_db(payload["symbol"], payload["start"], payload["end"])
        
        self.update_state(state="RUNNING", meta={"progress": 40})
        
        # 執行回測（Backtrader）
        result = run_hermes_backtest(df, payload.get("params", {}))
        
        self.update_state(state="RUNNING", meta={"progress": 80})
        
        # 存入 DB
        save_backtest_result(payload["strategy"], payload.get("params"), result)
        
        # 推送通知到 WebSocket
        import redis
        r = redis.Redis()
        r.publish("hermes_signals", json.dumps({
            "type": "BACKTEST_DONE",
            "result": result
        }))
        
        return result
    except Exception as e:
        self.update_state(state="FAILURE", meta={"error": str(e)})
        raise
```

#### Step 4：React 前端核心

jsx

```jsx
// components/BacktestPanel.jsx
import { useState, useEffect } from "react"
import { LineChart, Line, XAxis, YAxis, Tooltip, ResponsiveContainer } from "recharts"

export default function BacktestPanel() {
  const [params, setParams] = useState({
    symbol: "2330", market: "TW",
    start: "2022-01-01", end: "2024-12-31",
    ema_fast: 20, ema_slow: 60
  })
  const [status, setStatus] = useState("idle")
  const [result, setResult] = useState(null)

  const runBacktest = async () => {
    setStatus("running")
    const { task_id } = await fetch("/api/backtest/run", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(params)
    }).then(r => r.json())

    // Polling 等待結果
    const poll = setInterval(async () => {
      const data = await fetch(`/api/backtest/${task_id}`).then(r => r.json())
      if (data.status === "SUCCESS") {
        setResult(data.result)
        setStatus("done")
        clearInterval(poll)
      }
    }, 1500)
  }

  return (
    <div style={{ padding: 24 }}>
      {/* 參數設定 */}
      <div style={{ display: "flex", gap: 12, marginBottom: 24 }}>
        <input value={params.symbol} onChange={e => setParams({...params, symbol: e.target.value})}
               placeholder="股票代碼 (e.g. 2330)" />
        <input type="date" value={params.start}
               onChange={e => setParams({...params, start: e.target.value})} />
        <input type="date" value={params.end}
               onChange={e => setParams({...params, end: e.target.value})} />
        <button onClick={runBacktest} disabled={status === "running"}>
          {status === "running" ? "回測中..." : "執行回測"}
        </button>
      </div>

      {/* 績效摘要 */}
      {result && (
        <div style={{ display: "grid", gridTemplateColumns: "repeat(4,1fr)", gap: 12 }}>
          {[
            ["總報酬", `${result.return_pct.toFixed(2)}%`],
            ["Sharpe", result.sharpe?.toFixed(2) ?? "—"],
            ["最大回落", `${result.max_drawdown?.toFixed(2)}%`],
            ["交易次數", result.total_trades]
          ].map(([label, val]) => (
            <div key={label} style={{ background: "var(--surface-1)", padding: 16, borderRadius: 8 }}>
              <div style={{ fontSize: 12, color: "var(--text-secondary)" }}>{label}</div>
              <div style={{ fontSize: 22, fontWeight: 500 }}>{val}</div>
            </div>
          ))}
        </div>
      )}

      {/* 淨值曲線 */}
      {result?.equity_curve && (
        <ResponsiveContainer width="100%" height={260}>
          <LineChart data={result.equity_curve}>
            <XAxis dataKey="date" tick={{ fontSize: 11 }} />
            <YAxis />
            <Tooltip />
            <Line type="monotone" dataKey="value" stroke="#1D9E75" dot={false} strokeWidth={1.5} />
          </LineChart>
        </ResponsiveContainer>
      )}
    </div>
  )
}
```

#### Step 5：Docker Compose 一鍵啟動

yaml

```yaml
# docker-compose.yml
services:
  db:
    image: timescale/timescaledb:latest-pg16
    environment:
      POSTGRES_DB: hermes_quant
      POSTGRES_PASSWORD: password
    ports: ["5432:5432"]
    volumes: ["pgdata:/var/lib/postgresql/data"]

  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]

  api:
    build: ./backend
    command: uvicorn main:app --host 0.0.0.0 --port 8000 --reload
    depends_on: [db, redis]
    ports: ["8000:8000"]

  worker:
    build: ./backend
    command: celery -A celery_app worker --loglevel=info --concurrency=4
    depends_on: [db, redis]

  frontend:
    build: ./frontend
    ports: ["3000:3000"]
    depends_on: [api]

volumes:
  pgdata:
```

---

### 請 Agent 建構的正確方式

若要讓 Claude Code 或其他 Coding Agent 幫你完整建構，使用以下 prompt 結構：

```
# 任務：Hermes 量化交易 Web 系統

## 目標
建立完整的量化交易動態網頁系統，包含：
1. React 前端（策略設定、回測儀表板、訊號監控、K線圖）
2. FastAPI 後端（REST API + WebSocket）
3. Celery 工作佇列（非同步回測）
4. TimescaleDB 時序資料庫
5. Docker Compose 部署

## 規格
- 台股資料來源：AKShare（akshare 套件）
- 美股資料來源：yfinance
- 回測框架：backtrader，台股手續費 0.001425
- 訊號格式：HSP（Hermes Standard Packet）含 S/A/B/C 等級
- 風控：ATR停損 + 固定風險2% + 每日最大虧損3%

## 請執行步驟
1. 建立 docker-compose.yml
2. 建立 backend/ 目錄，初始化 FastAPI 專案
3. 實作 /api/backtest/run 和 /ws/signals 端點
4. 建立 frontend/ 目錄，初始化 React + Vite 專案
5. 實作回測儀表板組件（含 Recharts 淨值圖）
6. 撰寫 README.md 含啟動指令

## 限制
- 全程使用免費開源工具
- Python 套件不得使用付費 API
```