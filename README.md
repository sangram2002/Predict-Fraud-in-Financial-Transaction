# Predict-Fraud-in-Financial-Transaction
# Fraud Detection Project – Internship Task

**Data Science & Machine Learning**  
**International School of AI & Data Science (INSAID)**

---

## 📌 1. Data Cleaning: Missing Values, Outliers, and Multicollinearity

### Missing Values
- No null values in numeric columns.

### Outliers
- Large transaction `amount`s (up to millions) observed.
- Retained these as potential fraud signals.
['amount', 'oldbalanceOrg', 'newbalanceOrig', 'oldbalanceDest', 'newbalanceDest']  for all these columns non_fraud transactions were removed.

### Multicollinearity
- High correlation found between:
  - `oldbalanceOrg` & `newbalanceOrig`
  - `oldbalanceDest` & `newbalanceDest`

---

## 🤖 2. Model Description

### Model Used: `XGBoost Classifier`
- Also compared with Random Forest and GradientBoosting Classifier.

### Why XGBoost?
- Handles class imbalance well.
- Good interpretability.
- Robust to outliers & redundant features.

### Pipeline:
1. Categorical encoding of `type`

3. Train-test split using stratification.
4. Handled imbalance using RandomUnderSampler.
5. Model tuning with grid search & cross-validation.

---

## 📊 3. Feature Selection

### Methodology:
- Domain knowledge
- Correlation matrix
- Feature importance (XGBoost)

### Final Feature List:
- `type` (encoded)
- `amount`
- `oldbalanceOrg`, `newbalanceOrig`
- `oldbalanceDest`, `newbalanceDest`
- `balance_diff_org`, `balance_diff_dest`

Excluded: `nameOrig`, `nameDest` (identifiers, not useful)

---

## 📈 4. Model Performance

### Metrics Used:
- Precision, Recall, F1-score
- Out of all the 3 Recall is the main important metric here as we have to focus on minimizing fradulent activities i.e. minimizing False negatives Hence, maximizing Recall. 
- Confusion Matrix

### Performance Summary:
- **Precision:** 0.91
- **Recall:** 0.87
- **F1 Score:** 0.89
- **ROC-AUC:** 0.96

Focus was on **Recall**, to minimize false negatives.

---

## 🧠 5. Key Fraud Indicators

1. **Transaction Type**: `TRANSFER` and `CASH_OUT` most associated with fraud.
2. **Transaction Amount**: High values, especially > 200,000.
3. **Balance Gaps**: 
   - `balance_diff_org` being negative.
   - `oldbalanceDest = 0` and `newbalanceDest > 0`.

These features are consistent with account takeover and draining activity.

---

## ✅ 6. Do These Factors Make Sense?

**Yes:**
- Fraudsters often:
  - Transfer or withdraw large amounts.
  - Empty accounts (negative or near-zero balance after transaction).
  - Move funds to dormant or newly created accounts.

These behaviors reflect real-world fraud scenarios and are valid predictors.

---

## 🛡️ 7. Infrastructure Prevention Recommendations

1. **Real-time Transaction Monitoring**  
   Use ML models to flag suspicious activity instantly.

2. **High-Value Threshold Alerts**  
   Alert or block when transaction exceeds a defined limit.

3. **Two-Factor Authentication (2FA)**  
   Enforce 2FA for high-risk actions like TRANSFER and CASH_OUT.

4. **Device & IP Profiling**  
   Detect anomalies in access patterns.

5. **Behavioral Modeling**  
   Build customer transaction profiles to detect unusual behavior.

---

## 📉 8. Evaluating Effectiveness of Preventive Actions

### Post-Implementation KPIs:
- Drop in fraud detection rate (compare before & after)
- Change in false positive/negative counts
- Feedback from users (complaints, denials, support tickets)

### Method:
- **A/B Testing**: Apply changes to a subset of users and evaluate impact.
- **Continuous Monitoring**: Track performance metrics periodically to ensure model accuracy doesn’t degrade over time.

---

