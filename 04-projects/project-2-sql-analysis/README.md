# Project 2: SQL Analysis & Financial Database Design

**Timeline:** Weeks 5-6 (Month 2)
**Status:** Not Started

---

## Overview

Design and implement a relational database modelling a financial trading system — trades, positions, instruments, and P&L — and write complex analytical queries that answer real business questions a quant desk or data team would ask.

**What to Build:**
A database-driven analytics project that:
- Models a normalized trading/portfolio schema (instruments, trades, positions, P&L)
- Loads realistic sample market and trade data
- Implements complex analytical queries (rolling P&L, position sizing, drawdown)
- Creates reusable views for common finance analyses
- Includes data integrity constraints (no negative quantities, valid side checks)

---

## Learning Objectives

- Master SQL fundamentals (SELECT, JOIN, GROUP BY, window functions, CTEs)
- Learn database design and normalization (3NF)
- Practice writing complex analytical queries on time-series financial data
- Understand indexes and query optimization
- Implement domain-relevant data integrity constraints
- Prepare for Databricks DE Associate SQL component (Jan 2026)

---

## Tech Stack

**Core:**
- PostgreSQL (via Docker)
- SQL

**Supporting:**
- Python (data loading scripts)
- SQLAlchemy (ORM practice)
- DBeaver (database management)

**Development:**
- Docker (database container)
- Git for version control

---

## Schema Design

```
instruments       — ticker, name, asset_class, currency, exchange
trades            — trade_id, instrument_id, side (BUY/SELL), quantity, price, trade_date, account_id
positions         — account_id, instrument_id, net_quantity, avg_cost, as_of_date
accounts          — account_id, account_name, base_currency, account_type
daily_prices      — instrument_id, price_date, open, high, low, close, volume
corporate_actions — instrument_id, action_type, ex_date, ratio/amount
```

---

## Success Criteria

- [ ] Schema is properly normalized (3NF)
- [ ] At least 6 tables with proper FK relationships
- [ ] 10+ analytical queries covering finance use cases
- [ ] Window functions used for running P&L, rolling returns, rank by performance
- [ ] CTEs used for multi-step position reconciliation
- [ ] Appropriate indexes on trade_date, instrument_id, account_id
- [ ] Constraints: CHECK (side IN ('BUY','SELL')), NOT NULL on key fields
- [ ] Views: `vw_open_positions`, `vw_daily_pnl`, `vw_account_summary`
- [ ] Documentation explains schema design decisions
- [ ] Sample data: 5+ instruments, 500+ trades, 2 years of daily prices

---

## Project Structure

```
project-2-sql-analysis/
├── schema/
│   ├── 01_create_tables.sql
│   ├── 02_constraints.sql
│   └── 03_indexes.sql
├── data/
│   ├── sample_data/
│   │   ├── instruments.csv
│   │   ├── trades.csv
│   │   └── daily_prices.csv
│   └── load_data.py
├── queries/
│   ├── 01_basic_analytics.sql
│   ├── 02_window_functions.sql
│   ├── 03_pnl_analysis.sql
│   ├── 04_portfolio_metrics.sql
│   └── views.sql
├── docs/
│   ├── schema_diagram.png
│   └── query_explanations.md
├── README.md
└── docker-compose.yml
```

---

## Key Queries to Implement

1. **Daily P&L per account** — mark positions to market using daily_prices
2. **Cumulative realized P&L** — running total of closed trades
3. **Top 5 positions by unrealized gain/loss**
4. **Trade frequency by instrument and month** — GROUP BY + date truncation
5. **Win rate per instrument** — % of trades closed at profit
6. **Rolling 30-day volume** — window function
7. **Position reconciliation** — net quantity from raw trades vs positions table
8. **Max drawdown per account** — peak-to-trough on equity curve
9. **Sector/asset class breakdown** — JOIN with instruments
10. **Slippage analysis** — compare trade price vs same-day close price

---

## Resources

- PostgreSQL documentation: https://www.postgresql.org/docs/
- SQL for Finance: https://use-the-index-luke.com/
- Databricks SQL prep: https://docs.databricks.com/sql/
