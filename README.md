# 📊 gram-event-quant — Event-Study & Market Impact Analysis Pipeline

[![Python 3.13](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Tooling: uv](https://img.shields.io/badge/Package_Manager-uv-DE5FE9?style=for-the-badge&logo=astral)](https://docs.astral.sh/uv/)
[![Storage: DuckDB](https://img.shields.io/badge/Storage-DuckDB%20%2B%20Parquet-FFF000?style=for-the-badge&logo=duckdb&logoColor=black)](https://duckdb.org/)
[![Testing: Pytest](https://img.shields.io/badge/Tests-Pytest%20Suite-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white)](https://pytest.org/)

> **Quantitative Research Project:** An exploratory event-study framework designed to quantify short-term cryptocurrency market reactions following news and channel announcements in Telegram. Merges textual event signals with Bybit OHLCV market microstructure data to evaluate Abnormal Returns ($AR$) and Cumulative Average Abnormal Returns ($CAAR$).

---

## 🎯 Research Objective & Methodology

Traditional event studies measure whether specific events lead to statistically significant abnormal returns. This pipeline automates the entire econometric workflow:

```text
┌─────────────────────────┐       ┌─────────────────────────┐
│ Telegram Channels Feed  │       │   Bybit Market Data     │
│ (Telethon / MTProto)    │       │   (1m OHLCV Candles)    │
└────────────┬────────────┘       └────────────┬────────────┘
             │                                 │
             ▼                                 ▼
   ┌───────────────────┐             ┌───────────────────┐
   │  Ticker Resolver  │             │  DuckDB Storage   │
   │ & Ingestion Engine│             │ (Parquet Engine)  │
   └─────────┬─────────┘             └─────────┬─────────┘
             │                                 │
             └────────────────┬────────────────┘
                              ▼
               ┌──────────────────────────────┐
               │    Event Slicer & Window     │
               │   Estimation vs Event Span   │
               └──────────────┬───────────────┘
                              ▼
               ┌──────────────────────────────┐
               │  Econometrics: Market Model  │
               │  Abnormal Returns (AR/CAAR)  │
               └──────────────┬───────────────┘
                              ▼
               ┌──────────────────────────────┐
               │   Interactive HTML Reports   │
               │  & Event Hotness Dashboard   │
               └──────────────────────────────┘
```

* **Signal Ingestion:** Captures message timestamps, resolves mentions to tradeable spot/derivative tickers (`core/ticker_resolver.py`).
* **Microstructure Alignment:** Queries historical Bybit OHLCV data aligned around the event timestamp ($t_0$) within configurable pre-event and post-event windows.
* **Statistical Modeling:** Computes expected benchmark returns using a **Market Model**, extracts Abnormal Returns ($AR_t = R_{i,t} - \hat{R}_{i,t}$), and aggregates them into Cumulative Average Abnormal Returns ($CAAR$).
* **Visual Analytics:** Generates standalone interactive HTML reports and event hotness distribution dashboards for visual inspection.

---

## 🛠️ Key Components & Implementation

### 1. 🗄️ Columnar Storage with DuckDB & Parquet
* Utilizes **DuckDB** (`storage/duckdb_store.py`) and embedded Parquet files for fast out-of-memory queries over multi-gigabyte OHLCV candle datasets.
* Avoids heavyweight database overhead while maintaining sub-second analytical aggregations.

### 2. 📐 Statistical & Event-Study Engines
* **Event Slicing (`engine/event_slicer.py`):** Automatically slices time-series candles into estimation windows (baseline market beta) and event windows ($[-T_1, +T_2]$).
* **CAAR Engine (`stats/caar.py`):** Evaluates cross-sectional statistical significance of market reactions across all collected events.
* **Hotness Scoring (`engine/hotness.py`):** Quantifies event impact based on relative volume spikes and volatility expansion in the immediate post-announcement candles.

### 3. 📈 Automated Report Generation
* Outputs HTML dashboards (`data/processed/event_study_report.html`, `hotness_dashboard.html`).
* Allows visual tracking of mean trajectory curves, outlier trades, and event-window volatility.

---

## 🗂️ Project Structure

```text
gram-event-quant/
├── data/
│   ├── raw/                 # Raw candles in csv, tg-messages etc.
│   └── processed/           # Merged datasets & generated HTML research reports
├── scripts/
│   ├── auth_telegram.py     # MTProto session authentication
│   ├── render_dashboard.py  # Visualizer
│   └── run_analysis.py      # E2E pipeline script
├── src/
│   └── gram_quant/
│       ├── core/            # Config, Pydantic schemas, ticker resolver
│       ├── engine/          # Event slicer, ingestion pipelines, hotness metrics
│       ├── fetchers/        # Bybit & Telegram APIs
│       ├── stats/           # Market model estimation & CAAR statistics
│       ├── storage/         # DuckDB / Parquet integration layer
│       └── visualization/   # Plotly/Jinja render engines
├── tests/
│   ├── integration/         # Bybit, Telegram, and Excel ingestion tests
│   └── unit/                # CAAR math, slicer logic, ticker resolver tests
├── pyproject.toml           # Project metadata and dependencies
└── uv.lock                  # Deterministic dependency lockfile

```

---

## 👨‍💻 Author

**Arsenii Leno**  
*Software Engineering Student (FIIT STU Bratislava) & Law (UzhNU Faculty of Law)*

* 🌐 **Portfolio:** [arsenii-leno.github.io](https://arsenii-leno.github.io)
* 📑 **Workfolio:** [Notion Hub](https://bouncy-pyroraptor-569.notion.site/Workfolio-16c46a8dd0cd80f28fd6c43b2b604b21)
* 💬 **Telegram:** [@Arsen_Kozaque](https://t.me/Arsen_Kozaque)
* ✉️ **Email:** [xlenoa@stuba.sk](mailto:xlenoa@stuba.sk)

---

## ⚖️ Disclaimer

> This project is built purely for quantitative research, data engineering practice, and asynchronous pipeline design. It does not constitute financial advice, an endorsement of any digital asset, or an automated high-frequency trading system.
