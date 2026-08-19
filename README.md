# ⚡ gram-event-quant — Event-Driven Quantitative Trading Engine

**Status:** 🛠️ Active Development | **Language:** Python 3.11+ | **Architecture:** Async I/O

An asynchronous, event-driven trading engine and market data listener designed for low-latency execution and real-time exchange WebSocket processing.

---

## 🎯 System Architecture

┌─────────────────────────────────────────────────────────┐
│        Exchange WebSocket / REST API (Bybit / Binance)   │
├─────────────────────────────────────────────────────────┤
│            Async Event Loop (Python asyncio)            │
│  ├── Market Cache & Real-Time Orderbook Parsing         │
│  ├── Decimal Math & Precision Volume Calculator         │
│  └── Slippage Protection & Risk Checks                  │
├─────────────────────────────────────────────────────────┤
│            Order Execution & Structured Logging         │
└─────────────────────────────────────────────────────────┘


---

## 🚀 Core Mechanics

- **Non-Blocking I/O:** Built on `asyncio` and `aiohttp`/`websockets` for streaming real-time ticker and depth data.
- **Precision Math:** Uses Python `decimal` primitives to eliminate floating-point rounding errors during lot size calculations.
- **Risk Mitigation:** Built-in safeguards for slippage thresholds, max order size validation, and execution logging.

---

## 🛠️ Tech Stack

- **Core:** Python 3.11+, `asyncio`, `websockets`, `aiohttp`
- **Exchange Integration:** CCXT / Native Exchange REST & WebSocket APIs
- **Environment Management:** `python-dotenv`

---

## 🚀 Setup & Execution

```bash
# Setup virtualenv
git clone [https://github.com/arsenii-leno/gram-event-quant.git](https://github.com/arsenii-leno/gram-event-quant.git)
cd gram-event-quant
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure secrets (.env)
cp .env.example .env

# Run engine
python main.py
⚠️ Disclaimer
Educational and quantitative research software. Trading digital assets involves financial risk.
