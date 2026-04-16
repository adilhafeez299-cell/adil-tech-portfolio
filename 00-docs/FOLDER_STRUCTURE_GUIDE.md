# Folder Structure Guide
**Last Updated:** April 2026

---

## Repository Layout

```
data-engineering-journey/
├── 00-docs/          — Planning docs, cert tracks, master plan
├── 01-python/        — Python learning code
├── 02-aws/           — AWS cert study materials + labs
├── 03-linux/         — Linux learning materials
├── 04-projects/      — Portfolio projects
└── 05-job-search/    — CVs, job applications, strategy
```

---

## 00-docs/

Authoritative planning documents. Don't delete anything here without checking if it's still referenced.

| File | Purpose |
|---|---|
| `MASTER_PLAN_18MONTH.md` | 78-week roadmap overview, phase breakdown, cert timeline |
| `AWS_CERT_TRACK.md` | Detailed dual-cert plan (DVA-C02 + SAA-C03), study schedules, lab checklists |
| `CURRENT_STATUS.md` | Current week, active focus, cert status — update weekly |
| `PHILOSOPHY-AND-PRINCIPLES.md` | Core learning and job search philosophy — timeless |
| `FOLDER_STRUCTURE_GUIDE.md` | This file |
| `NEW_CHAT_STARTER_GUIDE.md` | Context template for starting Claude sessions — update as you progress |
| `RESOURCE_LIST.md` | Verified resources by phase |
| `WEEK_BY_WEEK_EXECUTION_18MONTH.md` | Detailed week-by-week breakdown |

---

## 01-python/

All Python learning code from the Bogdan Stashchuk course + Exercism exercises.

```
01-python/
├── Python_Course_Driven_Training/   — Bogdan course code, organised by topic
├── Python_practice_random/          — Misc practice scripts
├── Exercism_Execises/               — Exercism track exercises
└── projects/
    └── Project_1_file_organizer/    — Early Project 1 attempt (scratch)
```

**What goes here:** Course exercise code, Exercism solutions, scratch practice. Not portfolio code — that goes in `04-projects/`.

---

## 02-aws/

AWS cert study materials, notes, and lab code. Fill as you work through each cert.

```
02-aws/
├── developer-associate/     — DVA-C02 study (notes, labs, practice exam results)
└── solutions-architect/     — SAA-C03 study (notes, labs, architecture diagrams)
```

**Suggested structure per cert folder:**
```
developer-associate/
├── notes/
│   ├── ec2.md
│   ├── lambda.md
│   ├── iam.md
│   └── ...
├── labs/
│   ├── lab-01-ec2-setup/
│   ├── lab-02-s3-versioning/
│   └── ...
├── practice-exams/
│   └── results-log.md
└── weak-areas.md
```

---

## 03-linux/

Linux learning materials — command line, file system, permissions, scripting.

```
03-linux/
├── notes/          — Notes by topic (permissions, processes, scripting, etc.)
├── exercises/      — Shell scripting practice
└── cheatsheets/    — Quick reference
```

---

## 04-projects/

All portfolio projects. Each project has a detailed README with implementation plan.

```
04-projects/
├── project-1-python-automation/     — Python CLI data processor (Pandas)
├── project-2-sql-analysis/          — Financial trading schema + analytics
├── project-3-databricks-etl/        — Market data medallion pipeline
├── project-4-ml-classification/     — Fraud detection engine
├── project-5-aws-pipeline/          — Serverless AWS market data pipeline
├── project-6-capstone/              — Fintech Data Platform (integrates 1-5, 7-9)
├── project-7-options-pricing-engine/ — Black-Scholes + Greeks
├── project-8-algo-trading-backtester/ — SMA/RSI strategies, performance metrics
└── project-9-epidemic-simulator/    — SIR/SEIR models, Monte Carlo
```

**Rule:** Each project is a self-contained directory with `src/`, `tests/`, `README.md`, `requirements.txt`. Production-quality code only — no scratch work.

---

## 05-job-search/

```
05-job-search/
├── CVs/
│   ├── CV-universal.md      — ACTIVE CV (current version)
│   └── archive/             — Old CV versions (v1-v4)
├── COVER-LETTER-TEMPLATE.md
├── JOB-APPLICATION-STRATEGY.md
├── applications-tracker.csv
└── target-companies.csv
```

**CV policy:** `CV-universal.md` is the live version. Update it when certifications are earned or projects ship. Archive old versions rather than deleting.

---

## What Goes Where — Quick Reference

| Content | Location |
|---|---|
| Python course exercises | `01-python/Python_Course_Driven_Training/` |
| Exercism solutions | `01-python/Exercism_Execises/` |
| AWS cert notes and labs | `02-aws/developer-associate/` or `02-aws/solutions-architect/` |
| Linux notes | `03-linux/notes/` |
| Portfolio project code | `04-projects/project-N-*/src/` |
| Current CV | `05-job-search/CVs/CV-universal.md` |
| Job applications | `05-job-search/applications-tracker.csv` |

---

## Gitignore Notes

Covered: `__pycache__/`, `*.pyc`, `.env`, `*.csv` (data files), `data/`, `.idea/`

Add when needed:
- `*.log` — once projects generate logs
- `mlruns/` — once Project 4 MLflow is running
- `*.parquet`, `*.delta` — once Projects 3/5 produce output files
