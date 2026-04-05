# Hemoglobin Estimation from Face Videos

## Problem Description

This project aims to estimate blood hemoglobin levels from short face videos to make an alternative to blood tests. By extracting pixel intensity signals from six facial landmarks across the R, G, B, Cr, and C color channels, we analyze whether these raw optical signals carry enough information to predict hemoglobin values. The long-term goal is to enable low-cost anemia screening in settings without laboratory access.

## Dataset Source

Manually collected dataset consisting of 363 face videos (1–2 minutes each) recorded in front of a camera, paired with hemoglobin measurements stored in `info.xlsx` (column: `Deger`). The dataset is not publicly available. MediaPipe FaceLandmarker is used to locate six anatomical points (cene_sol, sol_yanak_ic3, sag_yanak_ic3, alin_ust_orta, dudak_ustu, cene_sag) and extract a 30×N multivariate time series per video (6 landmarks × 5 channels), saved as `.npy` files in `data/`.

## How the Three Projects Connect

This repository hosts all three deliverables of a single project on the same problem and dataset:

- **P1 — Problem Formulation & EDA**: Extracts raw channel signals from videos, explores signal patterns for a single video (time-series view of R/G/B/Cr/C per landmark), and aggregates mean/std summaries across all videos to study the relationship between raw channel statistics and hemoglobin.

The three deliverables share the same dataset and extraction pipeline; each builds on the findings of the previous stage.

## Repository Structure

```
.
├── p1/
│   └── p1_eda_<student_id>.ipynb     # P1 notebook (this deliverable)
├── videos/                            # Raw face videos (not included in repo)
├── data/                              # Extracted .npy files (30×N matrices)
├── info.xlsx                          # Hemoglobin labels
└── README.md
```
