# Model Card — Week 3 Baseline

# Model Card — ML Baseline System

## Problem
- **Target:** `is_high_value` (binary classification)
- **Unit of analysis:** one **user** (`user_id`) row in the feature table
- **Decision:** flag users likely to be “high value” so the business can prioritize outreach/retention actions (e.g., offers, account management)

---

## Data
- **Feature table path (project-relative):** `data/processed/features.csv`
- **Dataset hash (sha256 of feature table):**  
  `14cbce822b917a787133a2af7bd04699156f503177ea0494bbd7ac079f6a5b45`

---

## Splits
- **Holdout strategy:** random holdout
- **Parameters:** `test_size=0.2`, `seed=42`
- **Notes:** `group_col=null`, `time_col=null` (not grouped or time-based)

---

## Metrics (Holdout)
**Baseline vs Model (threshold = 0.5)**

| Metric     | Baseline | Model |
|------------|----------|-------|
| ROC AUC    | 0.50     | 1.00  |
| PR AUC     | 0.20     | 1.00  |
| Accuracy   | 0.80     | 1.00  |
| Precision  | 0.00     | 1.00  |
| Recall     | 0.00     | 1.00  |
| F1         | 0.00     | 1.00  |

- **Positive rate on holdout:** 0.20

---

## Limitations + Monitoring Sketch
### Limitations
- **Possible label leakage:** Perfect holdout scores suggest the target may be directly derivable from one or more features (e.g., `is_high_value` computed from spend-related fields), leading to overly optimistic evaluation.
- **Random split risk:** If data evolves over time, random splits may leak future information; consider time-based splits when applicable.
- **Small/demo dataset:** Results may not generalize without larger, real-world data.

### Monitoring (Production)
- **Schema checks:** validate inputs against `models/runs/<run_id>/schema/input_schema.json`.
- **Data drift:** track feature distribution drift (PSI/KS) and missingness rates.
- **Target drift:** monitor observed positive rate vs training/holdout (0.20).
- **Performance:** ROC AUC / PR AUC on fresh labels; precision/recall at business threshold.
- **Ops:** prediction volume, latency, error rate, invalid/unknown values.

---

## Reproducibility
- **Git commit:** `9e94790018da48b789e32ed94674401e381045d1`
- **Run ID:** `2025-12-31T23-00-02Z__classification__seed42`
- **pip freeze path:**  
  `models/runs/2025-12-31T23-00-02Z__classification__seed42/env/pip_freeze.txt`
