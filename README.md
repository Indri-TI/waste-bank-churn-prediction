# Customer Churn Prediction in Waste Banks Using XGBoost and SMOTE

Reproducibility repository for the manuscript *"Customer Churn Prediction in Waste Banks Using XGBoost and SMOTE"* (Jurnal RESTI). It contains the analysis code and a **de-identified** dataset that reproduce **all quantitative results** reported in the paper.

## Quick start

```bash
pip install pandas numpy scikit-learn imbalanced-learn xgboost lightgbm scipy matplotlib openpyxl
```

Open **`Reproduce_All_Results.ipynb`**, set the `PROJ` variable in the setup cell to this repository folder, and run all cells.

```python
PROJ = '.'                                        # if the notebook sits in the repo root
# Windows: PROJ = r'C:\path\to\this\repo'         # the r'...' prefix is required
# Colab:   see the optional upload cell in the notebook
```

All analyses use fixed seeds (`random_state=42`) and are fully deterministic.

## Repository structure

```
├── Reproduce_All_Results.ipynb      # Single notebook reproducing every result in the paper
├── 01_raw_data/
│   └── BUKU BESAR BANK SAMPAH LH.xlsx    # De-identified transaction log (sheet "Form Responses 1")
├── 02_clean_data/
│   └── dataset_churn_cleaned_bidang.xlsx # De-identified snapshot dataset (8,607 observations)
└── 03_experiment_split/
    ├── train_final_no_alamat.xlsx        # Customer-level training split (6,956 rows)
    └── test_final_no_alamat.xlsx         # Customer-level test split (1,651 rows)
```

## What the notebook reproduces

| Section | Content | Maps to |
|---|---|---|
| 2 | Main model performance, confusion matrix, ROC and PR curves | Table 1, Figures 3–4, 9 |
| 3 | Baseline model comparison (5 algorithms) | Table 2 |
| 4 | Churn-definition sensitivity (30/90/180-day windows) | Figures 7–8 |
| 5 | Hyperparameter optimization (randomized search) | Table 3 |
| 6 | Bootstrap 95% CIs, bootstrap AUC-difference and McNemar tests | Section 3.8 |
| 7 | Temporal forward-chaining validation | Section 3.9 |
| 8 | Permutation importance | Figure 10 |
| 9 | Probability calibration (Brier score, reliability curve) | Figure 11 |
| 10 | Fairness/subgroup analysis and multicollinearity (VIF) | Section 3.9 |
| 11 | Nested cross-validation | Section 3.9 |
| 12 | Computational cost (training and inference time) | Section 3.9 |

## Expected results

| Quantity | Value |
|---|---|
| ROC AUC (test) | 0.8985, 95% CI [0.881, 0.914] |
| Accuracy / Precision / Recall / F1 | 0.839 / 0.709 / 0.751 / 0.729 |
| MCC / Balanced Accuracy / Cohen's Kappa | 0.615 / 0.813 / 0.615 |
| Brier score | 0.116 |
| Baseline ranking (ROC AUC) | XGBoost 0.8985 > LightGBM 0.8975 > Random Forest 0.8963 > Logistic Regression 0.8794 > Decision Tree 0.8757 |
| XGBoost vs LightGBM / Random Forest | p ≈ 0.40 / 0.55 (not statistically different) |
| XGBoost vs Logistic Regression / Decision Tree | p < 0.001 (significant) |
| Temporal forward-chaining ROC AUC | 0.904 ± 0.031 (10 folds) |
| Nested cross-validation ROC AUC | 0.916 ± 0.011 |
| Variance inflation factors | all < 3.5 |
| Subgroup ROC AUC (male / female) | 0.881 / 0.938 |

Training and inference **times** depend on hardware and will differ from the values reported in the manuscript.

## Data privacy and de-identification

The original records are owned by the **Environmental Agency (Dinas Lingkungan Hidup) of Padang City** and contain personal information. The data published here have been **de-identified**:

- Account numbers were replaced with anonymous member IDs (`M0001`, `M0002`, …); the mapping is not distributed, so members cannot be re-identified. The customer-level grouping required for the leakage-aware evaluation is preserved.
- Member names, national identity numbers (NIK), addresses, and email addresses were removed.

The raw, identifiable records are **not shared** and remain with the corresponding author and the Environmental Agency.

## Citation

If you use this code or data, please cite the paper. Correspondence regarding the data may be directed to the corresponding author.
