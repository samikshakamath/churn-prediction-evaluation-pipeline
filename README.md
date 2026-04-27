# Customer Churn Evaluation and Modeling Framework – FoodCorp


 - **Author**: Samiksha Kamath 
- **Project Date**: 13/5/2025

This repository implements a **deployment-ready machine learning system** for predicting customer churn at FoodCorp on a **weekly operational cycle**.  
The objective is to proactively identify **currently active customers at high risk of imminent churn**, enabling timely and targeted retention interventions before disengagement becomes irreversible.

Unlike naïve inactivity-based churn definitions, this project adopts a **conditional, forward-looking churn framework** aligned with FoodCorp’s real-world retention strategy.
 
---

## Business Objective

The primary goal is to support FoodCorp’s marketing and retention teams by answering:

- Which *currently active* customers are most likely to churn in the next 30 days?
- What behavioural signals indicate early disengagement?
- How can customers be prioritised for intervention without excessive false positives?

To ensure operational relevance, churn is defined **only among recoverable customers**, not long-dormant users.

---

## Churn Definition

A customer is labelled as **churned** if:

- They made at least one purchase in the **previous 30-day input window**, and
- They made **no purchases in the subsequent 30-day output window**

This conditional definition ensures:
- Predictions target customers while intervention is still feasible
- The model focuses on behavioural change, not historical inactivity
- Alignment with FoodCorp’s weekly outreach cadence

At the prediction point, 967 active customers were evaluated, of which 406 churned  
(**churn rate: 41.99%**).

---

## Data and Feature Windowing

### Temporal Structure

Customer behaviour is captured using a **rolling 150-day behavioural history**, segmented into:

- Five non-overlapping **30-day input windows** (F5 → F1)
- One **30-day output window** for churn labelling

This mirrors real-world deployment, where only past data is available at prediction time.

---

## Feature Engineering

A hybrid feature schema was designed to balance **predictive power, interpretability, and scalability**.

### Lagged Features (per 30-day window)
- Spend (f1_spend → f5_spend)
- Quantity
- Visit frequency

### Aggregated Behavioural Features
- average_spend
- average_qty
- average_visit
- basket_value
- unit_cost

### Stability and Regularity Indicators
- mean_gap (average days between visits)
- frequency_variance
- spend_variance
- days_since_last_purchase

### Breadth and Context
- num_pro (number of unique products)
- store location indicators

This design captures **behavioural consistency and engagement patterns**, rather than relying purely on monetary value.

---

## Feature Selection Strategy

Feature selection was performed in two stages:

### 1. Correlation and Redundancy Analysis
- Strong negative correlation between churn and recent visit frequency
- Strong positive correlation between churn and mean_gap
- High multicollinearity across raw lag features (f1–f5)

### 2. Importance-Based Selection
Applied:
- Mutual Information (filter)
- Random Forest feature importance (embedded)
- Permutation importance (cross-validated)

Final features consistently highlighted:
- mean_gap
- frequency_variance
- average_visit
- days_since_last_purchase
- num_pro

This confirms churn is driven primarily by **behavioural irregularity**, not spend volume.

---

## Modeling Approach

### Candidate Models
- Random Forest
- Logistic Regression
- K-Nearest Neighbors
- Decision Tree
- Linear SVC
- Dummy Classifier (baseline)

### Model Selection Metric
- **F1 score** (primary)
- Precision, Recall, AUC (secondary)

F1 was prioritised to balance:
- capturing true churners (recall)
- avoiding excessive false positives (precision)

---

## Evaluation Strategy

A **temporally consistent rolling evaluation framework** was implemented:

- Rolling training and validation folds across five reference dates
- Strict prevention of data leakage
- Feature scaling applied using training data only
- Final evaluation on a fully held-out future period

This setup simulates real deployment conditions and ensures generalisation.

---

## Model Performance

Random Forest emerged as the best-performing model:

- **F1 Score:** 76%
- **AUC:** 87%
- **Precision:** 0.76
- **Recall:** 0.78
- **Accuracy:** 78%

The Dummy Classifier achieved F1 = 0%, AUC = 50%, confirming meaningful learning.

A classification threshold of **0.5** was selected to balance outreach efficiency and churn capture.

---

## Explainability and Insights

### SHAP Analysis

SHAP was applied to interpret the final Random Forest model.

Key drivers of churn:
- Low average_visit
- High mean_gap
- High frequency_variance
- Declining recent engagement

Low-impact features:
- average_spend
- monetary value metrics

This confirms that churn is primarily a **behavioural disengagement problem**, not a revenue problem.

---

## Customer Risk Segmentation

Customers were ranked by predicted churn probability and grouped into 10 risk bands.

- **High risk (Ranks 1–3):**  
  First-time or trial users, low frequency, high gaps, low product diversity  
  → Immediate re-engagement recommended

- **Medium risk (Ranks 4–6):**  
  Inconsistent regulars  
  → Nudges, bundles, loyalty incentives

- **Low risk (Ranks 7–10):**  
  Frequent, consistent, high diversity customers  
  → Retention and reward strategies

This ranking supports **precision targeting** and efficient resource allocation.

---

## Deployment Readiness

The pipeline is:
- Fully modular
- Automatable on a weekly cycle
- Scalable across FoodCorp’s store network
- Aligned with operational retention workflows

The model can be retrained and rescored weekly with minimal manual intervention.

---

## Tools and Technologies

- **SQL:** Feature extraction and aggregation
- **Python:** Modeling, evaluation, and explainability
- **Scikit-learn:** Pipelines and model tuning
- **SHAP:** Model interpretation
- **Pandas / NumPy:** Data processing

---
