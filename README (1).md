# Customer Churn & Revenue Impact Analysis (OTT Subscription)

An end-to-end churn analytics project in Python and SQL. It pulls data from a SQLite database, cleans and joins three tables, engineers churn features, and quantifies how much revenue is at risk, to recommend a targeted retention strategy.

## Business problem
An OTT subscription business wants to know **who is churning, why, and what it costs**, so it can focus retention effort where it matters most.

## Key findings
| Metric | Result |
|---|---|
| Overall churn rate | **28.6%** (6 of 21 customers) |
| Churn by contract type | **Monthly 55.6%** vs **Annual 8.3%**, 6.7x higher |
| Churn by plan | Basic 60%, Standard 22%, Premium 14% |
| Revenue at risk | **$73.94/month** MRR leakage from the 6 churned customers |
| CLTV erosion | **$2,047** across the 6 at-risk customers |
| Support escalations | 19% of customers escalated; escalations correlate strongly with churn (r = 0.77) |

**Recommendation:** run a contract-migration campaign that moves monthly subscribers to annual contracts. Prioritise Basic-plan customers and anyone with an escalated complaint.

## Dataset
SQLite database `customer_churn.db` with three tables:

| Table | Contents |
|---|---|
| `db_customer` | customer ID, name, country, state, gender, date of birth |
| `db_subscription` | start/renewal/cancellation dates, plan type, contract type, monthly charges, CLTV, churn score |
| `db_support` | complaint date, escalation flag, CSAT score |

> **Note:** this is a small sample (21 customers), so percentages move a lot with each customer. Treat the results as a demonstration of the analysis method, not as statistically conclusive findings.

## Workflow
1. **Connect and import:** read all tables from SQLite into pandas with `sqlite3` and SQL queries.
2. **Data cleaning**
   - Dropped empty or irrelevant columns (`pincode`, `interests`, `col_1`, `comment`).
   - Converted date columns to `datetime`.
   - Standardised gender values (`Men`/`Women` → `Male`/`Female`).
   - Filled missing countries from the state.
   - Removed duplicate complaints per customer so the join to support data doesn't duplicate rows.
3. **Feature engineering:** churn flag from the cancellation date, tenure in days, complaint count, churn-risk bands (Low/Med/High from churn score), and ordinal encoding of plan and contract type.
4. **Analysis (pandas, NumPy):** churn and retention rate, churn by plan, state and contract, ARPU, average tenure, revenue at risk, escalation rate, average complaints per user, and correlation of escalations with churn.
5. **Visualisation (Matplotlib, Seaborn):** monthly churn trend, churn by plan and state, correlation heatmap, pair plot, and a catplot of charges by plan, gender and risk.

## Tech stack
Python · SQL · SQLite (`sqlite3`) · Pandas · NumPy · Matplotlib · Seaborn · Jupyter Notebook

## Repository structure
```
├── Churn_Analysis.ipynb      # full analysis notebook
├── customer_churn.db         # source SQLite database
├── exported_churn_data.csv   # cleaned, merged dataset
└── README.md
```

## How to run
```bash
git clone https://github.com/Harshkm007/<repo-name>.git
cd <repo-name>
pip install numpy pandas matplotlib seaborn jupyter
jupyter notebook Churn_Analysis.ipynb
```

## Limitations and next steps
- The sample is small. A larger dataset would allow reliable segmentation and statistical testing.
- Correlation between escalations and churn does not prove causation.
- Next steps: add a churn prediction model (logistic regression or a tree-based model) and build a Power BI dashboard on the cleaned data.

## Author
**Harsh Kumar** · [LinkedIn](https://www.linkedin.com/in/harsh-kumar-192b65334/) · [GitHub](https://github.com/Harshkm007)
