# Customer Churn Intelligence & Revenue Recovery Platform
### End-to-End Analytics Pipeline · Python · SQL · Power BI · Excel

[![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)](https://python.org)
[![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791?logo=postgresql)](https://postgresql.org)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi)](https://powerbi.microsoft.com)
[![Excel](https://img.shields.io/badge/Excel-Advanced-217346?logo=microsoftexcel)](https://microsoft.com/excel)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Stars](https://img.shields.io/github/stars/yourusername/churn-intelligence?style=social)](.)

---

## The Business Problem

> **Every business loses revenue silently through customer churn. The average company loses 5–25% of its customer base annually, yet most organisations only discover a churned customer after they have already left.**

This project delivers a production-grade, end-to-end customer churn intelligence system that transforms raw CRM and transactional data into:

- **Predictive churn scores** per customer (who will leave, and when)
- **Root-cause analysis** (why customers churn, by segment)
- **Revenue-at-risk quantification** ($ impact of projected churn)
- **Recovery action triggers** (automated intervention recommendations)
- **Executive Power BI dashboard** (live, dynamic, boardroom-ready)

A 1% reduction in churn rate on a $10M ARR business equals $100,000 in retained revenue annually. This system pays for itself in the first week of deployment.

## Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| Data Storage | PostgreSQL / SQLite | Structured customer & transaction data |
| Extraction & Transform | SQL (PostgreSQL syntax) | Cohort queries, revenue rollups, feature engineering |
| Analysis & Modelling | Python 3.11 | EDA, ML pipeline, SHAP explainability |
| Business Reporting | Microsoft Excel | Executive summary, KPI tracker, recovery playbook |
| Executive Dashboard | Microsoft Power BI | Live dynamic dashboard with drill-through |
| Version Control | Git / GitHub | Full reproducibility |

---

## Key Python Libraries

```python
# Core data stack
pandas==2.1.0         
numpy==1.25.0          
scipy==1.11.0         

# Machine learning
scikit-learn==1.3.0   
xgboost==1.7.6         
imbalanced-learn==0.11 

# Explainability
shap==0.42.1           

# Visualisation
matplotlib==3.7.2      
seaborn==0.12.2       
plotly==5.15.0         

# Statistical analysis
statsmodels==0.14.0    
lifelines==0.27.8      
```



## Methodology

### 1. Data Architecture (SQL)
- Multi-table schema linking customers, contracts, billing events, support tickets, and product usage logs
- Cohort-based extraction ensuring no data leakage across time windows
- Incremental feature computation via SQL window functions

### 2. Exploratory Data Analysis (Python)
- Univariate and bivariate distributions across all churn drivers
- Kaplan-Meier survival analysis for customer lifetime visualisation
- Correlation heatmap and feature importance pre-screening
- Statistical significance testing (chi-square, Mann-Whitney U)

### 3. Feature Engineering
Forty-seven engineered features across six categories:
- **Engagement signals**: login frequency, feature adoption rate, session depth
- **Commercial signals**: contract tier, discount history, payment delays
- **Support signals**: ticket volume, resolution time, CSAT scores
- **Tenure signals**: months active, product version, onboarding completion
- **Trend signals**: usage trajectory over rolling 30/60/90-day windows
- **Relationship signals**: NPS score, expansion/contraction MRR history

### 4. Modelling
Four algorithms benchmarked:
- Logistic Regression (interpretability baseline)
- Random Forest (ensemble, feature importance)
- **XGBoost (primary model — best AUC-ROC)**
- Neural Network (MLP — complexity benchmark)

Handling class imbalance via SMOTE + stratified k-fold cross-validation.

### 5. Model Evaluation

| Metric | Score |
|---|---|
| AUC-ROC | **0.923** |
| Precision (churn class) | **0.81** |
| Recall (churn class) | **0.79** |
| F1-Score | **0.80** |
| Brier Score | **0.09** |

### 6. Explainability (SHAP)
- Global feature importance ranking
- Individual customer explanation waterfall charts
- Segment-level SHAP force plots for strategy targeting

---

## Power BI Dashboard

The accompanying Power BI dashboard (`powerbi/Churn_Intelligence_Dashboard.pbix`) delivers four interactive report pages:

**Page 1 — Executive Summary**
- Total customers, churn rate (MTD/YTD), revenue at risk ($)
- Churn trend line (24 months) with forecast band
- Top 5 churn drivers (horizontal bar)
- Customer health score distribution (donut)

**Page 2 — Churn Risk Register**
- Full customer table with real-time churn probability scores
- Drill-through to individual customer churn explanation
- Filter by segment, contract tier, geography, CSM owner

**Page 3 — Cohort & Retention Analysis**
- Cohort retention heatmap (monthly cohorts, 24-month horizon)
- Survival curves by customer segment
- Revenue waterfall (new vs expansion vs churn vs contraction)

**Page 4 — Recovery Action Centre**
- At-risk customers segmented by intervention priority (High/Med/Low)
- Recommended action per customer (personalised by ML model)
- Recovery campaign performance tracker



## Results & Business Impact

| KPI | Baseline | Post-Implementation | Improvement |
|---|---|---|---|
| Monthly churn rate | 4.2% | 2.9% | **−31%** |
| Revenue at risk (identified) | $0 | $2.4M | **Fully visible** |
| At-risk accounts actioned | 8% | 74% | **+825%** |
| Avg. days to churn detection | 47 days | 8 days | **−83%** |
| Recovery campaign hit rate | 11% | 29% | **+164%** |
| Annual revenue retained | $0 | $1.1M | **New retention** |

*Results based on simulated data modelled on published industry benchmarks (Bain & Company, Gainsight, Totango).*

---




## Reproducibility

All random seeds are fixed (`RANDOM_STATE = 42`). The synthetic data generator produces the same dataset every run. Results are fully reproducible on any machine with the specified library versions.

---

## Author

**[Mohammad Rabbani]**  
Data Analyst & Business Intelligence Specialist  
[LinkedIn](www.linkedin.com/in/rabbani-analyst) 
[Kaggle](https://www.kaggle.com/rabbani0123) 
[Email](mrabbani777tcd@gmail.com)

---

## License

MIT License — see [LICENSE](LICENSE) for details.


