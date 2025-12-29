# Model Card — Week 3 Baseline

## Problem
- Predict: is_high_value for User
- Decision enabled: Target high-value users with VIP offers or special marketing
- Constraints: CPU-only; offline-first; batch inference

## Data (contract)
- Feature table: data/processed/features.csv
- Unit of analysis: One row per User
- Target column: is_high_value, positive class: 1
- Optional IDs (passthrough): user_id
- Primary Metric: F1-Score

## Splits (draft for now)
- Holdout strategy: Random stratified split (75% train / 25% test)
- Leakage risks: "total_amount" might be a direct proxy for "is_high_value" (if high value is defined by spending > $100, this feature gives away the answer).

## Metrics (draft for now)
- Primary: F1-Score (Because the classes are likely imbalanced; we care about correctly finding the "1s" without too many false alarms).
- Baseline: Dummy Classifier (Majority Class strategy - i.e., predicting "0" for everyone).

## Shipping
- Artifacts: model.pkl, schema.json, metrics.json, test_data.parquet, requirements.txt
- Known limitations: Cold start problem (model will fail for new users with 0 orders/0 history).
- Monitoring sketch: Monitor the percentage of users predicted as "High Value" over time; watch for distribution shifts in "avg_amount".