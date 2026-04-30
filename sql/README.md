# SQL Layer
## Customer Churn Intelligence Platform

This folder contains the complete **PostgreSQL extraction, transformation, and feature engineering pipeline** — the data backbone of the churn intelligence model.

---

## File: `churn_queries.sql`

### Module Structure

| Module | Lines | Purpose |
|--------|-------|---------|
| **Module 1** — Schema Setup | 1–80 | Full 7-table schema: customers, contracts, billing_events, product_usage, support_tickets, nps_responses, churn_scores |
| **Module 2** — Feature Extraction | 81–240 | 8-CTE chain extracting all ML features: tenure, billing, engagement, support, NPS, revenue, risk scores |
| **Module 3** — Cohort Retention | 241–290 | Monthly cohort construction and retention rate grid |
| **Module 4** — Revenue at Risk | 291–340 | Expected loss quantification by risk tier, segment, industry |
| **Module 5** — Executive KPI Queries | 341–420 | 24-month churn trend, top 10 at-risk accounts, churn driver frequency |

---

## Schema Overview

```
customers (master)
    ├── contracts          (one active per customer)
    ├── billing_events     (payments, delays, refunds)
    ├── product_usage      (daily login & feature logs)
    ├── support_tickets    (ticket history + CSAT)
    ├── nps_responses      (latest NPS score per customer)
    └── churn_scores       (written by Python scoring pipeline)
```

---

## How to Run

### Prerequisites
- PostgreSQL 14+ (or compatible: Amazon RDS, Azure PostgreSQL, Google Cloud SQL)
- A database named `churn_db` (or rename in connection string)

### Step 1 — Create schema
```sql
-- Run Module 1 in your SQL client (psql, DBeaver, DataGrip, pgAdmin)
\i sql/churn_queries.sql
```

### Step 2 — Load synthetic data
```bash
python python/01_data_generation.py  # generates CSVs
psql -d churn_db -c "\copy customers FROM 'data/processed/customers.csv' CSV HEADER"
```

### Step 3 — Run feature extraction
```sql
-- Module 2 — outputs the full ML feature table
-- Copy output to data/processed/churn_features.csv
\COPY (SELECT * FROM final_features) TO 'data/processed/churn_features.csv' CSV HEADER;
```

### Step 4 — Run executive KPIs
```sql
-- Module 5 — for the Power BI dashboard
-- Run each query block and connect Power BI via DirectQuery
```

---

## Key Feature Engineering Highlights

### Composite Risk Score (SQL-computed)
```sql
ROUND(
    (recency_risk   * 0.25 +   -- days since last login
     payment_risk   * 0.25 +   -- payment delay history
     support_risk   * 0.25 +   -- ticket volume × resolution time
     sentiment_risk * 0.25)    -- NPS score normalised
    ::NUMERIC, 4
) AS composite_risk_score
```

### Window Functions Used
- `EXTRACT(MONTH FROM AGE(...))` — tenure computation
- `DATE_TRUNC('month', ...)` — cohort bucketing
- `FILTER (WHERE ...)` — conditional aggregation across time windows
- `DISTINCT ON (customer_id)` — latest-record selection (NPS)

---

## Query Performance Notes

| Query | Est. Runtime (10K customers) | Index Used |
|-------|---------------------------|------------|
| Feature extraction CTE | ~2.1s | idx_contracts_customer, idx_usage_customer_date |
| Cohort retention grid | ~0.8s | idx_billing_customer |
| Revenue at risk | ~0.3s | churn_scores.scored_at |
| Executive KPI trend | ~1.1s | idx_contracts_status |

All indexes are defined in Module 1 of the schema setup.

---

## Compatibility

| Platform | Compatible | Notes |
|----------|-----------|-------|
| PostgreSQL 14+ | ✅ | Native |
| Amazon RDS (PostgreSQL) | ✅ | Native |
| Google BigQuery | ⚠️ | Minor syntax changes needed (EXTRACT, INTERVAL) |
| SQL Server | ⚠️ | Replace `::NUMERIC` casts with `CAST()`, INTERVAL syntax differs |
| Snowflake | ✅ | Compatible with minor timestamp adjustments |
| SQLite | ❌ | Missing window functions and FILTER clause support |

---

*Queries written to PostgreSQL 15 standards. Tested on PostgreSQL 14.8 and 15.2.*
