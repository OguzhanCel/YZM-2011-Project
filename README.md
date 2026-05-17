# Hemoglobin Estimation from Face Videos

## Problem Description
This project aims to estimate blood hemoglobin levels from short face videos to make an alternative to blood tests. By extracting pixel intensity signals from six facial landmarks across the R, G, B, Cr, and C color channels, we analyze whether these raw optical signals carry enough information to predict hemoglobin values. The long-term goal is to enable low-cost anemia screening in settings without laboratory access.

## Dataset Source
Manually collected dataset consisting of 363 face videos (1–2 minutes each) recorded in front of a camera, paired with hemoglobin measurements stored in `info.xlsx` (column: `Deger`). The dataset is not publicly available. MediaPipe FaceLandmarker is used to locate six anatomical points (cene_sol, sol_yanak_ic3, sag_yanak_ic3, alin_ust_orta, dudak_ustu, cene_sag) and extract a 30×N multivariate time series per video (6 landmarks × 5 channels), saved as `.npy` files in `data/`.

## How the Three Projects Connect
This repository hosts all three deliverables of a single project on the same problem and dataset:

- **P1 — Problem Formulation & EDA**: Extracts raw channel signals from videos, explores signal patterns for a single video (time-series view of R/G/B/Cr/C per landmark), and aggregates mean/std summaries across all videos to study the relationship between raw channel statistics and hemoglobin.
- **P2 — Regression Modeling**: Builds on P1 features by engineering interaction terms, channel ratios, coefficients of variation, and gender-based features. Trains and compares Baseline LR, Multiple LR, Polynomial Regression, Ridge, and Lasso models. Best model (Lasso) achieves Test R² = 0.557 and MAE ≈ 0.70 g/dL. Lasso zeroed out 146/160 features, keeping only 14 informative ones.
- **P3 — Classification & Unsupervised Analysis**: Reframes the problem as binary anemia screening using WHO gender-specific thresholds (Hb < 12 g/dL for females, < 13 g/dL for males). Applies Lasso-based and mutual-information feature selection (160 → 23 features), PCA dimensionality reduction, K-Means and DBSCAN clustering, and trains six classifiers (Logistic Regression, KNN, Decision Tree, SVM, Random Forest, Gradient Boosting) with GridSearchCV. Best model (KNN) achieves Test F1 = 0.78 and AUC-ROC = 0.76.

The three deliverables share the same dataset and extraction pipeline; each builds on the findings of the previous stage.

## Summary of Findings Across P1–P3

| Deliverable | Key Result |
|-------------|------------|
| P1 — EDA | Narrow Hb range (10–14 g/dL), strong gender dimorphism, motion artifacts as main noise source |
| P2 — Regression | Lasso best: Test R² = 0.557, MAE = 0.70 g/dL; `is_male` is the strongest predictor (|r| = 0.76) |
| P3 — Classification | KNN best: Test Accuracy = 78%, F1 = 0.78, AUC = 0.76; anemic recall = 87% (only 5 missed) |

- **Gender** is overwhelmingly the strongest predictor across all stages — consistent with known sex-dimorphic hemoglobin reference ranges.
- **Chrominance ratios** (G/R, B/R) carry genuine physiological signal related to hemoglobin absorption.
- **Feature selection** from P2 (Lasso survivors) proved critical in P3 — eliminating numerical instability, reducing overfitting, and improving generalization.
- **Main limitation**: Per-video mean/std aggregation destroys the cardiac-band temporal structure (0.7–4 Hz) where the rPPG pulse signal lives. Frequency-domain and information-theoretic features are the natural next step.

## Repository Structure
```
.
├── p1/
│   └── p1_eda_24018029.ipynb             # P1 — EDA & signal exploration
├── p2/
│   └── p2_regression_24018029.ipynb      # P2 — regression modeling
├── p3/
│   └── p3_classification_24018029.ipynb  # P3 — classification & unsupervised
└── README.md
```
