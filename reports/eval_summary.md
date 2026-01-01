# Evaluation Summary

## What you trained
- **Model family:** Logistic Regression (binary classifier).
- **Preprocessing:** Scikit-learn `Pipeline` with:
  - Missing value imputation for numeric features.
  - One-hot encoding for categorical features.
  - Model trained end-to-end inside the pipeline to avoid train/test leakage.

---

## Results (Holdout)
**Baseline vs Model (threshold = 0.5)**

| Metric     | Baseline | Model |
|------------|----------|-------|
| ROC AUC    | 0.50     | 1.00  |
| PR AUC     | 0.20     | 1.00  |
| Accuracy   | 0.80     | 1.00  |
| Precision  | 0.00     | 1.00  |
| Recall     | 0.00     | 1.00  |
| F1         | 0.00     | 1.00  |

- The baseline predicts the majority class and fails to identify positives.
- The trained model achieves perfect performance on the holdout set.

---

## Error Analysis
- **Worst-case examples:** No misclassified samples on the holdout set; however, this is suspicious and indicates the task may be trivially separable.
- **Possible leakage checks:** The target (`is_high_value`) is likely derived directly from one or more input features (e.g., total spend), leading to label leakage and inflated metrics.
- **Next data fixes:** Redefine the target using future behavior only, remove any features used in target construction, and re-evaluate using a time-based split.

---

## Recommendation
- **Decision:** ❌ **Do not ship yet**
- **Rationale:** Although metrics are perfect, the results are not trustworthy due to strong evidence of label leakage and an overly optimistic random split. The model should only be reconsidered after fixing the target definition, removing leaking features, and validating performance on a realistic (time-based) holdout.
