# Project 5: AWS Market Data Pipeline

**Timeline:** Weeks 13-14 (Month 4)
**Status:** Not Started

---

## Overview

Build a serverless, event-driven market data pipeline on AWS. Raw OHLCV price data lands in S3, triggers Lambda for validation, gets catalogued by Glue, and is queryable via Athena for ad-hoc analytics. Infrastructure defined as code. Aligned with AWS DE Associate certification (May 2026).

**What to Build:**
An AWS-native data pipeline that:
- Ingests daily OHLCV price data and trade records to S3 (data lake)
- Triggers Lambda on S3 PUT events for validation and routing
- Runs Glue ETL jobs to transform raw → analytical layer
- Exposes clean data via Athena for SQL querying
- Monitors pipeline health with CloudWatch dashboards and alerts
- All infrastructure defined in CloudFormation / CDK

---

## Learning Objectives

- Master AWS data services: S3, Lambda, Glue, Athena, EventBridge, CloudWatch
- Implement serverless, event-driven architecture patterns
- Practice Infrastructure as Code (CloudFormation or CDK)
- Understand partitioning strategies for time-series financial data (by date/instrument)
- Configure IAM roles with least-privilege for data pipeline components
- Build toward AWS DE Associate certification (May 2026)

---

## Tech Stack

**Core:**
- AWS S3 (data lake — bronze/silver/gold prefixes)
- AWS Lambda (event-driven ingestion + validation)
- AWS Glue (ETL, data cataloging)
- Amazon Athena (SQL query layer)

**Supporting:**
- AWS CloudWatch (monitoring + alerts)
- AWS EventBridge (scheduling daily ingestion)
- AWS IAM (least-privilege roles)
- CloudFormation or AWS CDK (IaC)

**Development:**
- Python (Lambda functions, Glue scripts)
- SQL (Athena queries)
- Git for version control

---

## Architecture

```
[EventBridge] — daily schedule trigger
      ↓
[Lambda: ingest] — fetches OHLCV from yfinance, writes raw CSV to S3 bronze/
      ↓
[S3 Event] — PUT on bronze/ triggers validation Lambda
      ↓
[Lambda: validate] — schema check, price spike detection, routes to silver/
      ↓
[Glue ETL Job] — transforms silver/ → parquet, partitioned by date/instrument → gold/
      ↓
[Glue Data Catalog] — crawls gold/ and registers schema
      ↓
[Athena] — SQL queries against gold/ (daily returns, position summaries, P&L)
      ↓
[CloudWatch] — pipeline execution metrics, error alarms, cost dashboard
```

---

## S3 Bucket Structure

```
s3://market-data-lake-{account}/
├── bronze/
│   └── prices/year=2025/month=11/day=01/AAPL.csv
├── silver/
│   └── prices/year=2025/month=11/day=01/AAPL.parquet
├── gold/
│   └── daily_returns/year=2025/month=11/AAPL.parquet
└── glue-scripts/
    └── transform_silver_to_gold.py
```

---

## Success Criteria

- [ ] Daily ingestion runs on EventBridge schedule without manual intervention
- [ ] Lambda validates schema and rejects malformed files to a dead-letter prefix
- [ ] Glue job transforms bronze CSV → gold Parquet with date/instrument partitioning
- [ ] Glue Catalog automatically updated by crawler after each Glue run
- [ ] Athena can query 2+ years of price data in under 10 seconds
- [ ] CloudWatch alarm fires if Lambda error rate > 0 for 2 consecutive runs
- [ ] All infrastructure deployable from a single CloudFormation stack
- [ ] IAM roles follow least-privilege (no wildcard * permissions in production)
- [ ] Cost estimated and documented (target: <$5/month)
- [ ] Architecture diagram included in docs

---

## Project Structure

```
project-5-aws-pipeline/
├── infrastructure/
│   ├── cloudformation.yaml
│   └── parameters.json
├── lambda_functions/
│   ├── ingest_prices/
│   │   ├── handler.py
│   │   └── requirements.txt
│   └── validate_data/
│       ├── handler.py
│       └── requirements.txt
├── glue_scripts/
│   └── transform_silver_to_gold.py
├── athena_queries/
│   ├── create_tables.sql
│   ├── daily_returns.sql
│   └── portfolio_summary.sql
├── monitoring/
│   └── cloudwatch_dashboard.json
├── docs/
│   ├── architecture_diagram.png
│   └── setup_guide.md
└── README.md
```

---

## Key Athena Queries to Implement

1. Daily returns for all instruments in a date range
2. Top 10 instruments by 30-day volatility
3. Correlation matrix between instruments (rolling 90-day)
4. Missing data report — instruments with gaps in coverage
5. Cost monitoring query — S3 storage by prefix

---

## Resources

- AWS Glue: https://docs.aws.amazon.com/glue/
- AWS Lambda: https://docs.aws.amazon.com/lambda/
- Amazon Athena: https://docs.aws.amazon.com/athena/
- AWS DE Associate exam guide: https://aws.amazon.com/certification/certified-data-engineer-associate/
- AWS Well-Architected Framework (Data Analytics Lens)
