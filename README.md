# Telco Churn Analysis  

## Project Overview  
This project explores how different regression models handle a binary classification problem: **customer churn prediction** using the Telco Customer Churn dataset. The main focus is not only on prediction performance but also on **interpretability** and **assumption checking**.  

The models compared are:  
- **Linear Regression (OLS)** – included as a baseline, even though churn is binary.  
- **Logistic Regression** – the standard model for binary classification.  
- **Generalized Additive Models (GAMs)** – flexible models that allow nonlinear relationships.  

---

## Objectives  
1. Fit three regression-based models (OLS, Logistic, GAM) on the churn dataset.  
2. Evaluate their predictive performance using **Accuracy** and **AUC**.  
3. Perform and document **assumption checks** for each model.  
4. Visualize **feature effects** using heatmaps and smooth plots.  
5. Compare interpretability and highlight when different models are appropriate.  

---

## Methods  

### Preprocessing  
- Dropped non-predictive `customerID`.  
- Encoded categorical variables:  
  - Yes/No → 1/0  
  - Multi-class variables → One-Hot encoding  
- Cleaned `TotalCharges` (blanks → numeric, filled missing).  
- Dropped `TotalCharges` in final models due to multicollinearity with `tenure × MonthlyCharges`.  
- Scaled numeric features.  

### Models  
1. **Linear Regression (OLS)**  
   - Treated churn (0/1) as continuous.  
   - Reported R², MSE, Accuracy, AUC.  
   - Found assumption violations (heteroscedasticity, residual patterns).  

2. **Logistic Regression**  
   - Correct specification for binary outcomes.  
   - Reported Accuracy, AUC, classification metrics.  
   - Checked assumptions: linearity in logit, independence, multicollinearity, outliers.  

3. **Generalized Additive Model (GAM)**  
   - Logistic GAM with smooth splines.  
   - Captured nonlinear predictor effects.  
   - Reported Accuracy, AUC.  
   - Plotted smooth functions to interpret nonlinearities.  

---

## Assumption Checks  

- **Linearity**  
  - OLS: violated (residuals formed stripes).  
  - Logistic: tested with logit scatterplots.  
  - GAM: not required (nonlinear terms allowed).  

- **Independence of Observations**  
  - Durbin–Watson statistic ≈ 2 → no autocorrelation.  
  - Dataset design: one row per customer → assumption satisfied.  

- **Homoscedasticity**  
  - Breusch–Pagan p < 0.001 → violated in OLS.  
  - Not required for Logistic/GAM.  

- **Normality of Residuals**  
  - Violated in OLS (binary residuals, not normal).  
  - Not assumed in Logistic/GAM.  

- **Multicollinearity**  
  - Detected via VIF (TotalCharges highly collinear).  
  - Resolved by dropping `TotalCharges`.  

- **Influential Outliers**  
  - Cook’s distance showed no extreme influential points.  
  - A few mild cases, but no serious violations.  

---

## Results  

| Model                | Accuracy | AUC   | Notes |
|-----------------------|----------|-------|-------|
| Linear Regression     | ~0.79    | ~0.83 | Violates key assumptions, unstable for binary Y. |
| Logistic Regression   | ~0.74    | ~0.84 | Correct specification, stable, interpretable. |
| GAM                   | ~0.80    | ~0.85 | Best performance, captures nonlinearities. |

---

## Conclusion  
- Linear regression can produce decent AUC but violates key assumptions when applied to binary outcomes.  
- Logistic regression is the **correct baseline model** for churn: interpretable, robust, and theoretically appropriate.  
- GAM improves further by relaxing linearity assumptions and capturing nonlinear relationships in features.  
- Assumption checks confirm why Logistic and GAM are more suitable than OLS for this dataset.  
