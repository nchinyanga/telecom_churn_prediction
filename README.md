# Telecom Customer Churn Prediction & Retention Strategy

**Predicting which customers will churn, quantifying the revenue at risk, and translating the model into a business retention strategy.**


---

## Overview

Nearly half of the customers in this 100,000-account telecom dataset churned within the observed period  **49.6%**, representing **$2.88M in monthly recurring revenue** ($34.6M annualized) at risk. This project builds an end-to-end pipeline that:

1. Merges usage/billing and account-level demographic data into a single 100,000 × 100 customer view
2. Engineers 20 behavioral features (usage trend, cost efficiency, service friction, lifetime value)
3. Selects the 25 most predictive, non-redundant features from 145+ candidates
4. Build and train a Catboost model
5. Translates the model's output into a **business proposal** with a driver-matched retention playbook and revenue-impact estimates

This isn't just a modeling exercise, the goal was to answer the question a business stakeholder actually asks: *"If we build this, what do we do with it and what's its worth?"*

## Key Results

| Metric | Value |
|---|---|
| Customers analyzed | 100,000 |
| Churn rate | 49.6% |
| Monthly revenue at risk | $2.88M |
| Annualized revenue exposure | $34.6M |
| Model | CatBoost |
| Model recall (churners correctly flagged) | 91.4% |
| Model AUC | 0.681 |
| Est. monthly revenue protected (10% campaign success) | $288,000 |

## What Drives Churn

Cross-checked with two independent feature-importance methods (mutual information + Random Forest importance), five drivers stood out:

| Driver | Finding |
|---|---|
| **Equipment age & tenure** | Churn rises from 45.3% (<1yr handset) to 60.1% (3–5yr handset), the single strongest signal |
| **Usage trend ratios** | Declining recent-vs-historical usage is a leading indicator of disengagement |
| **Cost / pricing efficiency** | Cost-per-minute relative to plan matters more than raw bill size |
| **Service call-failure rate** | Dropped/blocked calls are a direct, low-cost-to-act-on service signal |
| **Revenue & lifetime-value trend** | The top revenue quartile alone accounts for $1.39M of the $2.88M at risk |


## From Model to Strategy

The full [business proposal](docs/Churn_Retention_Business_Proposal.docx) translates these findings into a driver-matched retention playbook, for example:

- **Aging equipment**  automated upgrade offers triggered at 2 years, before risk peaks
- **Declining usage**  early re-engagement bundles for customers trending down
- **High cost-per-minute**  automated plan-fit reviews and bill-shock prevention
- **High call-failure rate**  proactive service recovery and network quality follow-up
- **High-value at-risk accounts**  a dedicated white-glove retention track

It also includes a phased rollout plan (deploy → integrate → pilot → measure & scale) and a revenue-protected estimate across a range of campaign success rates.

📄 **[Read the full business proposal ](docs/Churn_Retention_Business_Proposal.docx)**
📓 **[Explore the full analysis notebook ](notebooks/Telecom_churn_prediction.ipynb)**

## Repository Structure

```
├── notebooks/
│   └── Telecom_churn_prediction.ipynb   # Full analysis: EDA, feature engineering, modeling
├── docs/
│   └── Churn_Retention_Business_Proposal.docx   # Business case & retention strategy
├── images/
│   ├── feature_importance.png
│   ├── confusion_matrix.png
│   └── revenue_protected.png
└── README.md
```

## Approach

1. **Data merging**  joined usage/billing records with account demographics on `Customer_ID`, validated for key uniqueness and overlap before merging
2. **Cleaning**  median imputation for skewed numeric fields, explicit "Unknown" category for missing categoricals, z-score-based winsorizing for outliers (chosen over IQR after IQR proved unreliable on zero-inflated columns)
3. **Feature engineering** 20 engineered ratios covering cost efficiency, service quality, usage momentum and customer lifetime value
4. **Feature selection**  combined mutual information and Random Forest importance, then dropped features with > 0.85 pairwise correlation to reduce redundancy
5. **Modeling**  5-fold stratified cross-validation; CatBoost selected and threshold-tuned for recall
6. **Business translation**  converted model output into churn-driver analysis, revenue-at-risk quantification and a concrete retention strategy

## Tech Stack

`Python` · `pandas` / `numpy` · `scikit-learn` · `CatBoost`  · `matplotlib` / `seaborn` · `Jupyter`




