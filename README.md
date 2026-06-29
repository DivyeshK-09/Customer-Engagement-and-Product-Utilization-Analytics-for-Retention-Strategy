# Customer Retention Intelligence — European Bank

> **Unified Mentor Internship Project** | Mentored by the European Central Bank  
> Analyzing customer churn through behavioral engagement and relationship depth — not demographics.

---

## Overview

Banks traditionally assess retention risk through demographic signals: account balance, salary, credit score. This project challenges that assumption.

Using a dataset of 10,000 European bank customers across France, Germany, and Spain, we demonstrate that **behavioral signals — engagement status, product depth, and relationship tenure — are stronger and more actionable predictors of churn** than any financial demographic in the dataset.

The project delivers a full analytical pipeline: from raw data validation through KPI design, engagement segmentation, and a live interactive dashboard — with a companion research paper and executive summary for policy stakeholders.


https://uf4499jn4tewwbty6txncx.streamlit.app/


---

## Key Findings

| Finding | Detail |
|---|---|
| **Active members retain 12.6 points better** | 85.7% retention vs 73.2% for inactive — the single largest gap of any variable |
| **Two products is the retention sweet spot** | Churn drops from 27.7% (1 product) to 7.6% (2 products) — then rises sharply to 82.7% at 3 and 100% at 4 |
| **49.1% of high-balance customers are inactive** | This disengaged-premium segment churns at 32.3% — well above the 20.4% bank-wide average |
| **Credit card ownership has no retention effect** | Stickiness score ≈ 1.0 — a genuine negative finding, reported honestly |
| **RSI predicts churn monotonically** | Customers scoring 80–100 on the Relationship Strength Index churn at 10.8%; those scoring 0–39 churn at 33.2% |

---

## Project Structure

```
├── app.py                              # Main dashboard — Engagement vs Churn Overview
├── utils.py                            # Data loading, validation, KPI formulas
├── pages/
│   ├── 1_Product_Utilization.py        # Product depth impact analysis
│   ├── 2_Disengaged_Customer_Detector.py  # High-value at-risk customer detector
│   └── 3_Retention_Strength_Scoring.py # RSI scoring panel
├── European_Bank.csv                   # Dataset (10,000 customers)
├── requirements.txt
└── .streamlit/
    └── config.toml                     # Theme configuration
```

---

## Dashboard Modules

### 1. Engagement vs Churn Overview (`app.py`)
The landing page. Global sidebar filters (geography, activity status, product count, balance, salary, age) cascade across all modules. Shows engagement profile segmentation across four behavioral archetypes, the active vs. inactive headline comparison, and all five top-level KPIs.

### 2. Product Utilization Impact Analysis
The product-depth churn curve — including the non-monotonic cliff at 3–4 products. Breaks down single vs. multi-product retention and cross-analyses product count against activity status.

### 3. High-Value Disengaged Customer Detector
Adjustable percentile thresholds for balance and salary. Surfaces customers who are financially significant but behaviorally absent — the silent churn risk pool. Includes salary-balance mismatch detection (high earners with zero balance at this bank) and a downloadable at-risk customer list.

### 4. Retention Strength Scoring
The Relationship Strength Index (RSI) — a composite 0–100 score per customer combining activity, product depth, tenure, and card ownership. Includes an interactive "score a profile" panel with a live gauge chart, plus the full RSI distribution with churned/retained overlay.

---

## KPI Definitions

All formulas are implemented in `utils.py` and documented to match the companion research paper exactly.

**Engagement Retention Ratio**
```
Retention rate (active members) ÷ Retention rate (inactive members)
Result: 1.17× — active members retain meaningfully better
```

**High-Balance Disengagement Rate**
```
Share of above-median-balance customers who are inactive
Result: 49.1% — nearly half of premium customers show no active engagement
```

**Credit Card Stickiness Score**
```
Retention rate (card holders) ÷ Retention rate (non-holders)
Result: 1.008× — effectively no effect; reported as a negative finding
```

**Relationship Strength Index (RSI)**
```
(IsActiveMember × 40)
+ (min(NumOfProducts, 2) / 2 × 30)
+ (Tenure / 10 × 15)
+ (HasCrCard × 15)
= Score from 0–100
```

> Product benefit is deliberately **capped at 2** in the RSI formula. Churn rises sharply at 3–4 products in this dataset — rewarding product accumulation beyond 2 would make the index reward a behavior associated with departure, not loyalty. See the research paper for full rationale.

---

## Dataset

| Field | Description |
|---|---|
| `CustomerId` | Unique customer identifier |
| `Surname` | Customer surname |
| `CreditScore` | Customer creditworthiness (350–850) |
| `Geography` | France, Spain, or Germany |
| `Gender` | Male / Female |
| `Age` | Customer age (18–92) |
| `Tenure` | Years with the bank (0–10) |
| `Balance` | Account balance (€0–€250,898) |
| `NumOfProducts` | Number of bank products held (1–4) |
| `HasCrCard` | Credit card ownership (0/1) |
| `IsActiveMember` | Active engagement indicator (0/1) |
| `EstimatedSalary` | Estimated annual salary |
| `Exited` | Churn indicator — target variable (0/1) |

**10,000 customers. 20.4% overall churn rate. No missing values.**

---

## Running Locally

### Requirements
- Python 3.12 (Streamlit does not yet support Python 3.14)
- pip

### Setup

```bash
# Clone the repo
git clone https://github.com/your-username/customer-retention-intelligence.git
cd customer-retention-intelligence

# Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the dashboard
streamlit run app.py
```

Open [http://localhost:8501](http://localhost:8501) in your browser.

### Python 3.14 users (Fedora 43 etc.)

Streamlit's protobuf dependency is not yet compatible with Python 3.14. Use pyenv to run the app under 3.12:

```bash
pyenv install 3.12.9
pyenv local 3.12.9
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

### Verify it's working

Once loaded, confirm these values with no filters applied:

| Metric | Expected |
|---|---|
| Total customers | 10,000 |
| Overall churn rate | 20.4% |
| Engagement Retention Ratio | 1.17× |
| High-Balance Disengagement Rate | 49.1% |
| Credit Card Stickiness Score | 1.01× |

---

## Deliverables

- [x] Streamlit dashboard (4 modules, live analytics)
- [ ] Research paper (EDA, KPI analysis, recommendations) — in progress
- [ ] Executive summary for policy stakeholders — in progress

---

## Methodology

```
Data Ingestion & Validation
    → Schema check, binary field consistency, churn label audit

Engagement Classification
    → Four behavioral profiles: Active Engaged / Active Low-Product /
      Inactive High-Balance / Inactive Disengaged

Product Utilization Analysis
    → Churn curve by product count, single vs. multi-product retention,
      identification of the 3-4 product churn cliff

Financial Commitment vs. Engagement Analysis
    → Balance × activity cross-analysis, salary-balance mismatch detection,
      at-risk premium customer identification

Retention Strength Assessment
    → RSI design and validation, churn by RSI tier,
      engagement threshold identification
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data processing | Python, pandas, numpy |
| Visualisation | Plotly Express, Plotly Graph Objects |
| Dashboard | Streamlit |
| Dataset | CSV (European_Bank.csv) |

---

## About

**Project:** Customer Retention Intelligence  
**Program:** Unified Mentor Data Analytics Internship  
**Mentor:** European Central Bank  
**Author:** Nakama (Computer Vision & Data Engineer)  
**Institution:** KIIT University, B.Tech Computer Science, 2025  
