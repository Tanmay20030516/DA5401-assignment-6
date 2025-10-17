# **DA5401 A6: Imputation via Regression for Missing Data**

**Name:** Tanmay Gawande  
**Roll Number:** DA25M030

## **Overview**

This project analyzes the impact of different missing data handling strategies on binary classification model performance. We compare three imputation methods — **Median Imputation**, **Linear Regression Imputation**, and **Non-Linear Regression Imputation** — which retain the full dataset size, against **Listwise Deletion**, which maximizes data quality by retaining only complete cases.


## **Dataset & Context**
* Dataset: [Kaggle](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset)
* **Initial Dataset Size:** 30,000 observations
* **Missingness:** Approximately **23%** of the dataset contained missing values
* **Data Retention:**

  * Imputed Models (**A**, **B**, **C**) used ≈ 30,000 samples
  * Model **D** (Listwise Deletion) used ≈ 23,000 complete samples


## **Model Performance Metrics**

The table below summarizes the key classification metrics (after threshold optimization):

| Method | Imputation Type       | Accuracy | Precision | Recall | F1-score |
| ------ | --------------------- | -------- | --------- | ------ | -------- |
| A      | Median Imputation     | 0.7987   | 0.5540    | 0.4597 | 0.5025   |
| B      | Linear Regression     | 0.7990   | 0.5551    | 0.4597 | 0.5029   |
| C      | Non-Linear Regression | 0.7865   | 0.5186    | 0.4823 | 0.4998   |
| D      | Listwise Deletion     | 0.8145   | 0.6015    | 0.4620 | 0.5226   |


## **Key Insights**

### **1. Why Imputed Models Underperform**

Despite retaining more data (≈ 30,000 observations), the imputed models (A, B, C) show three critical weaknesses:

* **Noise Introduction:**
  Imputation adds uncertainty, diluting the signal-to-noise ratio — especially problematic when ≈ 23% of the dataset was imputed.

* **Model Instability Under Threshold Changes:**
  The noticeable accuracy drops in imputed models (particularly Model C: −2.2%) after threshold optimization indicate that they learned **spurious feature relationships** that collapse when decision boundaries shift.

* **Precision–Recall Trade-off Failure:**
  Model C’s precision dropped drastically (**0.6933 → 0.5186**, −17.5%), revealing poor class discrimination — a typical symptom of optimizing for **quantity over quality**.


### **2. Linear vs. Non-Linear Imputation (Model B vs. C)**

When comparing regression-based imputation approaches:

* **Model B (Linear Regression Imputation)** consistently outperformed **Model C (Non-Linear Regression Imputation)** across all major metrics.

#### **Why Linear Performed Better — The Role of Misspecification**

Model B’s superior performance suggests that the true relationship between the imputed feature and its predictors was **approximately linear**.
Model C’s non-linear regression likely **overfit** to random noise, assuming complexity that didn’t exist, leading to:

* False correlations,
* Higher variance,
* Greater instability, and
* Poorer generalization.


## Project Structure

```
DA5401-assignment-6/
├── da25m030-assignment-6-solution.ipynb    # Main Jupyter Notebook
├── requirements.txt                        # Project requirements
└── README.md                               # Project documentation
```


## **Conclusion and Recommendation**

### **Best Strategy (Overall): Listwise Deletion (Model D)**

If maintaining the full sample size is required, **Model B (Linear Regression Imputation)** is the preferred imputation strategy.
It yields slightly higher **F1-score (0.5029)** and greater stability than both the **Median (Model A)** and **Non-Linear (Model C)** imputations.

However, the **best overall strategy** for this scenario is **Listwise Deletion (Model D)** — provided that removing ≈ 23% of rows does **not introduce sampling bias**, i.e., if missingness is close to *Missing Completely At Random (MCAR)*.


### **Justification**

* **Performance Superiority:**
  Model D achieved the highest **Accuracy (0.8145)** and **F1-score (0.5226)** among all methods.

* **Conceptual Soundness:**
  The decline in performance for imputed datasets underscores that **loss of data quality** (via artificial imputations) can outweigh the benefits of larger sample size.
  Training on complete, verified data allowed Model D to learn more **robust and reliable patterns**.


