# BT4012 Fraud Detection — Kaggle Competition 2026

## Overview

This repository contains the implementation, experiments and final submission pipeline for the BT4012 Fraud Detection Kaggle Competition 2026.

The task is to identify fraudulent Bitcoin transactions using anonymised transaction features and graph information. The dataset is temporally partitioned, with training data covering time steps 1–35 and test data covering time steps 36–49.

The official evaluation metric is **ROC-AUC**. Since the test period occurs after the training period, the modelling approach focuses on chronological validation and generalisation to future transactions.

## Repository Structure

```text
bt4012-fraud-detection/
├── data/
│   └── raw/
│       ├── train.csv
│       ├── test.csv
│       ├── txs_edgelist.csv
│       └── sample_submission.csv
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_baseline_models.ipynb
│   ├── 03_hyperparameter_tuning.ipynb
│   ├── 04_ensemble_optimisation.ipynb
│   ├── 05_advanced_graph_features.ipynb
│   ├── 06_chronological_stacking.ipynb
│   ├── 07_full_stack_pseudo_labelling.ipynb
│   └── 08_robust_view_bagging.ipynb
├── Final/
│   └── A0282609X.ipynb
├── final_outputs/
├── reports/
├── submissions/
├── requirements.txt
├── .gitignore
└── README.md
```

The `notebooks/` directory documents exploratory analysis, model development and experimental comparisons. The `Final/` directory contains the consolidated pipeline for reproducing the two selected Kaggle submissions.

The raw datasets and generated submission CSV files are stored locally and excluded from version control.

## Methodology

### Exploratory Data Analysis

Initial analysis investigates class imbalance, missing values, feature distributions and changes in transaction characteristics across time steps.

The transaction edge list is also examined to understand the network structure and identify opportunities for graph-based feature engineering.

### Model Development

The modelling process explores several approaches:

- Gradient boosting models, including HistGradientBoosting (HGB), CatBoost and LightGBM.
- Hyperparameter tuning and ensemble weight optimisation.
- Graph-based feature engineering using transaction relationships.
- Chronological stacking with a logistic regression meta-model.
- Pseudo-labelling using high-confidence predictions from unlabelled transactions.
- Alternative feature representations and model combinations.
- Robust view-bagging with training-window diversification and rank-based aggregation.

The final submissions use two different ensemble strategies.

### Chronological Stacking

Chronological stacking combines predictions from HistGradientBoosting, CatBoost and LightGBM using a logistic regression meta-model.

Base-model predictions are generated using historical training and validation windows, allowing the meta-model to learn how to combine their outputs without training directly on the evaluation period.

This approach aims to exploit complementary model behaviour while respecting the temporal ordering of transactions.

### Robust View-Bagging

The robust view-bagging approach introduces diversity across both feature representations and training periods.

Three feature views are constructed:

1. **Raw:** Original anonymised transaction features.
2. **Graph:** Raw features supplemented with graph-based structural and neighbourhood information.
3. **Full:** Graph features supplemented with within-time-step rank and standardised feature transformations.

Models are trained using both the complete available labelled history and a more recent training window. This allows the ensemble to combine information from longer-term patterns and more recent transaction behaviour.

Candidate predictions are evaluated through forward-chaining validation. The final ensemble applies model filtering and view-balanced rank averaging to reduce dependence on any single feature representation or model configuration.

This strategy aims to improve resilience to temporal distribution shift by combining diverse predictions rather than relying on one model or feature set.

### Temporal Validation

Chronological validation is used to assess performance on transactions occurring after the training period.

This better reflects the competition's train-test separation than a conventional random train-validation split.

**ROC-AUC** is the official competition metric. **PR-AUC** is also examined during model development because fraudulent transactions represent a minority class.

The two final pipelines retain their original validation and model-selection procedures rather than imposing a single selection rule across all experiments.

## Final Models and Results

Two submissions were selected based on their public leaderboard performance.

| Submission | Method | Public ROC-AUC |
|---|---|---|
| `sub_14_robust.csv` | Robust view-bagging ensemble | **0.95076** |
| `experiment18_stacking.csv` | Chronological stacking | **0.94914** |

Robust view-bagging achieved the highest public leaderboard ROC-AUC among the selected submissions.

Chronological stacking was retained as the second submission because it provides a distinct modelling strategy based on a learned meta-model.

These scores reflect the public leaderboard only. Performance on the private leaderboard may differ.

## Reproducing the Final Submissions

### Environment Setup

The final notebook is intended to run in a Python 3.12 environment.

Clone the repository:

```bash
git clone https://github.com/charlllene/bt4012-fraud-detection.git
cd bt4012-fraud-detection
```

Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

The main modelling libraries include pandas, NumPy, scikit-learn, CatBoost, LightGBM and SciPy.

### Dataset Setup

Download the competition datasets from Kaggle and place them in `data/raw/`:

```text
data/raw/
├── train.csv
├── test.csv
├── txs_edgelist.csv
└── sample_submission.csv
```

The raw data files are excluded from version control.

### Running the Final Notebook

Open:

```text
Final/A0282609X.ipynb
```

Select the project's Python virtual environment as the Jupyter kernel and execute all cells in order.

The notebook independently runs the chronological stacking and robust view-bagging pipelines, preserving their original training and validation procedures.

The generated submission files are saved to `final_outputs/`:

```text
final_outputs/
├── experiment18_stacking.csv
└── sub_14_robust.csv
```

The notebook also validates the submission schema, transaction order, row counts and prediction values.

### Reproducibility Verification

The notebook includes an optional comparison against the original Kaggle submission CSV files stored in `submissions/`.

For each submission, the comparison checks column structure, transaction alignment and numerical differences between the original and regenerated predictions.

The comparison uses an absolute tolerance of \(10^{-12}\). This check should be performed after executing the updated final notebook.

## Limitations

The primary challenge is temporal distribution shift between the training and test periods. Patterns learned from historical transactions may not generalise to later time steps.

The anonymised features limit interpretability, while class imbalance creates additional challenges for model evaluation.

Graph-based and time-relative feature engineering may introduce unstable signals across periods. Ensemble diversification reduces reliance on individual components but does not guarantee robustness under future distribution shifts.

Public leaderboard performance is measured on only a subset of the competition test data, so it may not accurately represent final private leaderboard performance.

## Project Information

**Module:** BT4012 — Fraud Detection

**Institution:** National University of Singapore

**Academic Year:** 2026/2027 Semester 1

**Student Number:** A0282609X
