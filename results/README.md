# 📊 Results

This folder contains the exported model outputs used to generate all evaluation visualizations in the paper.

## File: `results_export.json`

Generated from the notebook after training all five models. Contains the following keys:

| Key | Type | Description |
|---|---|---|
| `class_distribution` | dict | Count of each class label in the test set |
| `pca_coords` | list[list] | 2D PCA projection of TF-IDF test features (800 × 2) |
| `pca_labels` | list[int] | True labels for each test instance (same order as `pca_coords`) |
| `predictions` | dict | Hard predictions (0 or 1) for each model on the test set |
| `probabilities` | dict | Predicted probabilities (class 1) for each model on the test set |
| `confusion_matrices` | dict | 2×2 confusion matrix for each model |
| `feature_importance` | dict | Top 20 TF-IDF feature names → XGBoost importance scores |

### Example structure

```json
{
  "class_distribution": {"0": 422, "1": 378},
  "predictions": {
    "XGBoost": [0, 1, 1, 0, ...],
    "Random Forest": [0, 1, 0, 0, ...]
  },
  "probabilities": {
    "XGBoost": [0.12, 0.89, 0.76, ...],
    ...
  },
  "confusion_matrices": {
    "XGBoost": [[405, 17], [41, 337]],
    ...
  },
  "feature_importance": {
    "was": 0.094,
    "both": 0.081,
    ...
  }
}
```

---

## Reproducing the Results

To regenerate `results_export.json` from scratch, run the full notebook:

```bash
jupyter notebook notebooks/hallucination_detection.ipynb
```

The export script at the end of the notebook saves this file automatically.

---

## Key Findings Summary

| Model | Accuracy | AUC |
|---|---|---|
| Logistic Regression | 90.38% | 0.956 |
| CatBoost | 92.00% | 0.960 |
| LightGBM | 92.25% | 0.950 |
| Random Forest | 92.50% | 0.967 |
| **XGBoost** | **92.75%** | 0.956 |

- **XGBoost** achieves the highest accuracy (92.75%)
- **Random Forest** achieves the highest AUC (0.967)
- **XGBoost ↔ LightGBM** predicted probability correlation: **0.99** (near-identical decision patterns)
- **Top features** driving classification: "was", "both", "is", "the", "in" — function words, not content words, pointing to stylistic pattern detection rather than semantic fact verification
