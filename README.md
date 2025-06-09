# 🧠 Customer Churn Evaluation and Modeling Framework – FoodCorp


 - **Author**: Samiksha Kamath 
- **Project Date**: 13/5/2025

This repository presents a structured, data-driven system for defining and evaluating customer churn in the context of transactional retail analytics. The approach goes beyond static churn labeling, introducing a dynamic, behaviorally derived churn framework. The project evaluates operational thresholds, models risk behavior, and simulates churn detection to support data-driven retention strategies.

---

## 📌 Analytical Objectives

- Redefine churn using time-sensitive behavioral rules based on inter-transaction gaps.
- Identify optimal inactivity threshold (β) through statistical distribution analysis.
- Estimate churn impact using population-level Predicted Churn Class Counts (PCCC).
- Simulate real-time deployment logic using forward-facing churn flagging mechanics.
- Provide a lightweight, interpretable system for integration with CRM/marketing workflows.

---

## 📈 Methodology Summary

### 🧩 Churn Definition
A customer is flagged as churned if:
- (current_date - last_transaction_date) > β
- - β is a tunable threshold informed by inter-visit gap quantiles.
- Strict inequality ensures proper reactivation and one-time churn flagging.

### 🔍 Evaluation Pipeline
1. **Inter-Visit Distribution Analysis**  
   Quantifies customer frequency patterns to support evidence-based β selection.

2. **Churn Impact Estimation (PCCC)**  
   For each β value, calculates expected number of customers classified as churned.

3. **Rolling Labeling Simulation**  
   Forward simulation across time to verify:
   - No repeated churn flags
   - Reactivation is respected
   - Labeling is operationally viable

---

## 📁 Repository Structure

- **`Churn_Prediction_Model_Evaluation.html`**  
  Interactive statistical evaluation with distributional analysis and threshold comparisons

- **`Churn_Prediction_Final_Model_Implementation.html`**  
  Simulated churn flagging logic with forward-facing labeling mechanics

- **`Supporting Documentation_ Churn Prediction System.pdf`**  
  Theoretical foundations, assumptions, and methodology overview

- **`ConsultingCorp_Report_ml76.pdf`**  
  Formal client-facing report with insights, recommendations, and model outcomes

- **`FoodCorp_Churn_Modeling_and_Evaluation_Report.pdf`**  
  Executive-level summary of project findings and implications

---

## 🧪 Tools & Libraries

- **Python** (pandas, numpy, matplotlib)
- **HTML Reports** (interactive visual analysis)
- **PDF Documents** (business/academic reporting)

---

## 💡 Strategic Value

This churn modeling framework supports:
- Data-driven targeting of high-risk customers
- Threshold optimization based on real customer behavior
- Transparent simulation of churn detection in production
- Scalable, low-latency deployment in marketing pipelines

_This repository bridges statistical rigor and operational feasibility to deliver a deployable churn prediction pipeline aligned with business strategy._
