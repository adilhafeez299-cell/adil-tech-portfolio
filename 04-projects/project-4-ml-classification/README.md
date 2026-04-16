# Project 4: Fraud Detection Engine

**Timeline:** Weeks 11-12 (Month 3)
**Status:** Not Started

---

## Overview

Build an end-to-end fraud detection ML pipeline on transaction data. Fraud detection is one of the most common ML problems in fintech — it involves imbalanced classification, precision-recall tradeoffs, feature engineering on transaction sequences, and threshold tuning. Strong signal to fintech employers.

**What to Build:**
An ML pipeline that:
- Processes raw transaction data and engineers fraud-relevant features
- Handles severe class imbalance (fraud is ~0.1-1% of transactions)
- Trains and compares multiple classifiers
- Optimizes for precision-recall tradeoff (not just accuracy)
- Tunes classification threshold for business cost function
- Tracks experiments with MLflow
- Outputs a scoring pipeline that accepts new transactions

---

## Learning Objectives

- Understand imbalanced classification and why accuracy is a misleading metric for fraud
- Practice feature engineering on transaction data (velocity features, time-based features)
- Implement SMOTE and class-weighting to handle imbalance
- Learn precision-recall tradeoffs and threshold tuning
- Build a reproducible ML pipeline with MLflow experiment tracking
- Demonstrate domain knowledge directly relevant to fintech roles

---

## Tech Stack

**Core:**
- Python 3.11+
- Pandas, NumPy
- scikit-learn
- imbalanced-learn (SMOTE)

**Supporting:**
- MLflow (experiment tracking)
- Matplotlib, Seaborn (visualization)
- Jupyter notebooks (exploration)
- pytest

**Development:**
- Git for version control
- conda: dataeng environment

---

## Dataset

**Primary:** [Kaggle Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
- 284,807 transactions, 492 frauds (0.17% positive rate)
- Features are PCA-transformed (V1–V28) + Amount + Time
- Real anonymized European cardholder data from 2013

**Alternative:** Synthetic transaction data generated with realistic patterns if Kaggle not available.

---

## Success Criteria

- [ ] Class imbalance handled (SMOTE and/or class_weight='balanced')
- [ ] Baseline: Logistic Regression with class weighting
- [ ] Random Forest and XGBoost/Gradient Boosting implemented
- [ ] Evaluation uses Precision, Recall, F1, AUC-ROC, AUC-PR (not just accuracy)
- [ ] Precision-Recall curve plotted for all models
- [ ] Threshold tuning implemented with cost-sensitive analysis
- [ ] Feature importance analysis completed
- [ ] MLflow tracks all experiments (params, metrics, artifacts)
- [ ] Scoring pipeline accepts new transaction records
- [ ] 80%+ test coverage on pipeline components
- [ ] Final model achieves AUC-PR > 0.75

---

## Project Structure

```
project-4-fraud-detection/
├── src/
│   ├── __init__.py
│   ├── data_processing.py
│   ├── feature_engineering.py
│   ├── model_training.py
│   ├── model_evaluation.py
│   ├── threshold_tuning.py
│   └── scoring_pipeline.py
├── notebooks/
│   ├── 01_exploratory_analysis.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_experimentation.ipynb
├── tests/
│   ├── test_preprocessing.py
│   └── test_features.py
├── models/
│   └── .gitkeep
├── data/
│   ├── raw/
│   └── processed/
├── config/
│   └── config.yaml
├── mlruns/
│   └── .gitkeep
├── README.md
└── requirements.txt
```

---

## Implementation Plan

### Phase 1 — EDA & Baseline
- Explore class distribution, feature distributions, Amount/Time patterns
- Baseline: predict all transactions as non-fraud → measure why accuracy is misleading
- Logistic Regression with `class_weight='balanced'` as first real model

### Phase 2 — Feature Engineering
- `velocity_features`: count of transactions by card in last 1h, 24h, 7d
- `amount_zscore`: normalized amount relative to card's historical average
- `time_features`: hour of day, day of week, time since last transaction

### Phase 3 — Imbalance Handling
- SMOTE oversampling on training set only (never on validation/test)
- Compare: no resampling vs SMOTE vs class weighting

### Phase 4 — Model Training & Comparison
- Logistic Regression, Random Forest, XGBoost
- Cross-validation with `StratifiedKFold`
- MLflow logging for each experiment

### Phase 5 — Threshold Tuning
- Plot Precision-Recall curve
- Define cost function: `cost = FP_cost * FP + FN_cost * FN`
- Find optimal threshold that minimizes expected cost

### Phase 6 — Scoring Pipeline
- `scoring_pipeline.py`: accepts DataFrame of new transactions → returns fraud probability + binary prediction

---

## Key Concepts

**Why accuracy fails:** If 0.17% are fraud, predicting "not fraud" always gives 99.83% accuracy but catches 0 frauds.

**AUC-PR vs AUC-ROC:** For imbalanced data, AUC-PR is more informative — it focuses on the minority (fraud) class.

**Threshold tradeoff:**
- Low threshold → high recall (catch more fraud), but more false positives (block legit transactions)
- High threshold → high precision (fewer false alarms), but miss more fraud
- Business sets the cost ratio: blocking a legit transaction costs ~$5; missing a fraud costs ~$80

---

## Resources

- Kaggle dataset: https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud
- imbalanced-learn: https://imbalanced-learn.org/
- MLflow: https://mlflow.org/docs/
- scikit-learn: https://scikit-learn.org/
