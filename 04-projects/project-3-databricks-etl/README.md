# Project 3: Databricks Market Data ETL Pipeline

**Timeline:** Weeks 9-10 (Month 3)
**Status:** Not Started

---

## Overview

Build a production-grade ETL pipeline on Databricks that ingests raw market and trade data, transforms it through the medallion architecture, and produces clean analytical datasets for portfolio analytics. Directly aligned with the Databricks DE Associate certification (Jan 2026).

**What to Build:**
A medallion architecture pipeline that:
- Ingests raw OHLCV price data and trade records (CSV / API)
- Cleans and conforms data through Bronze → Silver → Gold layers
- Implements data quality checks (price spikes, missing bars, duplicate trades)
- Produces Gold-layer aggregates: daily P&L, position snapshots, return series
- Orchestrates with Databricks Workflows
- Uses Delta Lake for all storage layers

---

## Learning Objectives

- Master PySpark fundamentals (DataFrames, transformations, actions)
- Understand distributed processing on financial time-series data
- Implement medallion architecture with Delta Lake
- Practice data quality validation on real-world messy market data
- Learn Databricks Workflows for pipeline orchestration
- Build directly toward Databricks DE Associate (Jan 2026) and Professional (Feb 2026)

---

## Tech Stack

**Core:**
- Databricks Community Edition
- PySpark
- Delta Lake
- Python

**Supporting:**
- SQL (Gold layer transformations)
- yfinance or Alpha Vantage (data source)
- Azure Blob Storage or DBFS

**Development:**
- Databricks Notebooks
- Databricks Workflows
- Git for version control

---

## Pipeline Architecture

```
[Raw Sources]
  yfinance API → OHLCV CSVs
  Trade records → CSV files

[Bronze Layer] — raw ingestion, schema applied, load timestamp added
  bronze_ohlcv        — raw price bars
  bronze_trades       — raw trade records

[Silver Layer] — cleaned, validated, deduplicated
  silver_prices       — validated OHLCV, outliers flagged, gaps filled
  silver_trades       — deduplicated, side/quantity validated

[Gold Layer] — business-level aggregates
  gold_daily_returns  — daily % return per instrument
  gold_position_snapshot — daily net positions per account
  gold_pnl_summary    — daily realized + unrealized P&L per account
```

---

## Success Criteria

- [ ] Pipeline ingests OHLCV data for 10+ instruments over 2+ years
- [ ] Implements full medallion architecture (Bronze/Silver/Gold)
- [ ] Uses Delta Lake format at all layers
- [ ] Data quality checks: price spike detection (>10% 1-day move flagged), duplicate trade removal, missing bar detection
- [ ] Implements incremental processing (process only new data on each run)
- [ ] Gold layer produces correct daily returns and P&L
- [ ] Orchestrated via Databricks Workflow with dependency ordering
- [ ] Parameterized notebooks (date range, instrument list)
- [ ] Comprehensive logging at each layer
- [ ] Documentation includes architecture diagram

---

## Project Structure

```
project-3-databricks-etl/
├── notebooks/
│   ├── 01_ingest_bronze.py
│   ├── 02_transform_silver.py
│   ├── 03_aggregate_gold.py
│   └── utils/
│       ├── data_quality.py
│       └── helpers.py
├── config/
│   ├── pipeline_config.json
│   └── schema_definitions.py
├── workflows/
│   └── market_data_workflow.json
├── tests/
│   └── test_transformations.py
├── docs/
│   ├── architecture_diagram.png
│   └── pipeline_design.md
└── README.md
```

---

## Implementation Plan

### Bronze — Raw Ingestion
- Load CSVs/API response to Delta table
- Add `_load_timestamp`, `_source_file` metadata columns
- No transformations — preserve raw data exactly

### Silver — Clean & Validate
- Remove duplicate OHLCV bars (same instrument + date)
- Flag price spikes: `abs(close/prev_close - 1) > 0.10`
- Fill missing trading days (forward-fill or mark as missing)
- Validate trades: `quantity > 0`, `side IN ('BUY', 'SELL')`, `price > 0`

### Gold — Business Aggregates
- `daily_returns`: `(close - prev_close) / prev_close`
- `position_snapshot`: net quantity from cumulative trade history
- `pnl_summary`: `(close - avg_cost) * net_quantity` per account/instrument/day

---

## Resources

- Databricks documentation: https://docs.databricks.com/
- Delta Lake guide: https://docs.delta.io/
- Databricks DE Associate exam guide: https://www.databricks.com/learn/certification
- PySpark documentation: https://spark.apache.org/docs/latest/api/python/
