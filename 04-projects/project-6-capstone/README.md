# Project 6: Fintech Data Platform (Capstone)

**Timeline:** Weeks 15-16 (Month 4)
**Status:** Not Started

---

## Overview

A production-quality, end-to-end fintech data platform that combines every skill from the learning path. Ingests live/historical market data, runs it through a cloud data lake, prices options positions, generates trading signals, and serves a portfolio analytics reporting layer. This is the centrepiece portfolio project — demonstrating data engineering, cloud, ML, and quant finance domain knowledge in one system.

**What to Build:**
A unified data platform that:
- Ingests daily OHLCV data + options chains from public APIs (yfinance, CBOE)
- Stores and processes data through a medallion architecture (S3 + Delta/Parquet)
- Prices options positions using the Black-Scholes engine (Project 7)
- Runs trading signal generation using the backtester framework (Project 8)
- Produces a daily portfolio report: positions, P&L, Greeks exposure, signal summary
- Orchestrated end-to-end on AWS (Project 5 architecture, extended)
- Queryable via Athena + served through a lightweight Python API or CLI dashboard

---

## Learning Objectives

- Integrate Python, SQL, cloud, PySpark, ML, and quant finance skills into one system
- Design a production-grade data architecture under realistic constraints
- Implement CI/CD for a data pipeline project
- Build a portfolio-ready showcase demonstrating end-to-end DE + fintech domain knowledge
- Practice presenting technical architecture decisions to a non-technical audience

---

## Tech Stack

**Data Ingestion & Storage:**
- yfinance / CBOE API (market data)
- AWS S3 (data lake)
- Delta Lake / Parquet

**Processing:**
- PySpark / Databricks (batch transformations)
- Python (options pricing, signal generation)
- AWS Glue (ETL orchestration)

**Analytics & Serving:**
- Amazon Athena (SQL layer)
- Python CLI dashboard (Rich library) or FastAPI endpoint

**Infrastructure:**
- AWS Lambda + EventBridge (event-driven ingestion)
- CloudFormation / CDK (IaC)
- GitHub Actions (CI/CD)

**Dev Tools:**
- MLflow (experiment tracking for signal models)
- pytest + coverage
- Docker (local development)

---

## System Architecture

```
[Market Data Sources]
  yfinance API → OHLCV, options chains
  CBOE → implied volatility surface

[Ingestion Layer — AWS Lambda + EventBridge]
  Daily trigger → fetch data → land in S3 bronze/

[Processing Layer — AWS Glue / PySpark]
  Bronze → Silver: validate, clean, deduplicate
  Silver → Gold: daily returns, position snapshots, IV surface

[Analytics Engines — Python]
  Black-Scholes Pricer → options P&L, Greeks per position
  Signal Generator → SMA crossover + RSI signals per instrument
  Fraud/Anomaly Check → flag unusual price/volume patterns

[Reporting Layer]
  Athena SQL queries → portfolio summary, P&L attribution
  Python CLI dashboard → daily report: positions, Greeks, signals
  Export: CSV + JSON reports to S3 output/

[Monitoring]
  CloudWatch → pipeline health, data freshness alerts
  GitHub Actions → CI on every push (lint, test, integration)
```

---

## Success Criteria

- [ ] End-to-end pipeline runs daily without manual intervention
- [ ] Options positions priced with full Greeks (delta, gamma, theta, vega)
- [ ] At least 2 trading signals generated per instrument (SMA + RSI)
- [ ] Daily portfolio report produced: P&L, Greeks exposure, signal summary
- [ ] All infrastructure deployable from IaC (CloudFormation/CDK)
- [ ] CI/CD pipeline configured (GitHub Actions: lint → test → deploy)
- [ ] Athena queries return results in < 10 seconds on 2 years of data
- [ ] Test coverage > 70% on core processing logic
- [ ] Architecture documented with clear diagram
- [ ] Demo-able in 10-15 minutes with live data or replay mode
- [ ] GitHub README is portfolio-ready (problem statement, architecture, demo)

---

## Project Structure

```
project-6-capstone/
├── infrastructure/
│   ├── cloudformation.yaml
│   └── cdk/
├── ingestion/
│   ├── lambda_ingest/
│   │   └── handler.py
│   └── data_sources/
│       ├── yfinance_loader.py
│       └── cboe_loader.py
├── processing/
│   ├── glue_scripts/
│   │   ├── bronze_to_silver.py
│   │   └── silver_to_gold.py
│   └── engines/
│       ├── options_pricer.py      # wraps Project 7
│       └── signal_generator.py   # wraps Project 8
├── analytics/
│   ├── athena_queries/
│   │   ├── portfolio_summary.sql
│   │   ├── pnl_attribution.sql
│   │   └── greeks_exposure.sql
│   └── reporting/
│       ├── daily_report.py
│       └── dashboard.py
├── tests/
│   ├── unit/
│   └── integration/
├── .github/
│   └── workflows/
│       └── ci.yml
├── docs/
│   ├── architecture_diagram.png
│   ├── setup_guide.md
│   └── technical_design.md
├── docker-compose.yml
├── README.md
└── requirements.txt
```

---

## Implementation Phases

### Phase 1 — Foundation (Days 1-2)
- Define instrument universe (10-20 equities + their options chains)
- Design data model end-to-end
- Set up AWS infrastructure via CloudFormation
- Configure GitHub Actions CI skeleton

### Phase 2 — Data Pipeline (Days 3-5)
- Implement ingestion Lambdas (OHLCV + options chains)
- Build Bronze → Silver → Gold Glue jobs
- Validate pipeline with 1 instrument before scaling

### Phase 3 — Analytics Engines (Days 6-8)
- Integrate Black-Scholes pricer (from Project 7)
- Integrate signal generator (from Project 8)
- Wire both into Gold layer output

### Phase 4 — Reporting & Polish (Days 9-12)
- Build Athena query layer
- Build daily report CLI dashboard
- Add CloudWatch monitoring + alerts
- Write integration tests

### Phase 5 — Documentation & Demo (Days 13-14)
- Architecture diagram
- Portfolio-ready README
- Record demo walkthrough

---

## Portfolio Presentation

### README Must Include:
1. Problem statement (why this platform exists)
2. Architecture diagram
3. Technology choices with rationale
4. Setup instructions (< 10 commands to deploy)
5. Example output (screenshot of daily report)
6. Key engineering decisions and tradeoffs
7. Future improvements

### Demo Script (10-15 min):
1. Problem overview — why a fintech data platform? (2 min)
2. Architecture walkthrough (3 min)
3. Live run: trigger pipeline, show S3 layers, query Athena (4 min)
4. Daily report output: positions, Greeks, signals (3 min)
5. CI/CD + monitoring demo (2 min)

---

## Resources

- All previous projects (1-5, 7-9) — this capstone integrates them
- AWS Well-Architected Framework
- Databricks DE Professional exam prep
- https://github.com/topics/algorithmic-trading (reference implementations)
