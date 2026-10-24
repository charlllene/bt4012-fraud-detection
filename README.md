# BT4012 Fraud Detection — Kaggle Competition 2026

## Overview

This repository contains the implementation and experiments for the BT4012 Fraud Detection Kaggle Competition 2026.

The task is to classify fraudulent Bitcoin transactions using anonymised transaction features and graph information. The dataset is temporally partitioned, with training data covering time steps 1–35 and test data covering time steps 36–49.

The primary evaluation metric is **ROC-AUC**. Since the test period occurs after the training period, the modelling approach focuses on temporal validation and generalisation to future transactions.

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
│   ├── 08_diverse_model_stacking.ipynb
│   ├── 09_time_relative_features.ipynb
│   ├── 10_feature_engineering_anonymised.ipynb
│   └── 11_spearman.ipynb
├── Final/
│   └── A0282609X.ipynb
├── final_outputs/
├── reports/
├── submissions/
├── requirements.txt
├── .gitignore
└── README.md
```

The `notebooks/` directory contains the exploratory experiments and model development process. The `Final/` directory contains the consolidated notebook used to reproduce the selected Kaggle submissions.

The raw datasets and generated submission files are stored locally and are not intended to be committed to GitHub.

## Methodology

### Exploratory Data Analysis

Initial analysis examines the distribution of transaction features, class imbalance, missing values and temporal changes in transaction patterns.

The transaction edge list is also explored to understand the available graph structure.

### Model Development

The experiments investigate several approaches:

- Gradient boosting models, including HistGradientBoosting and CatBoost.
- Hyperparameter tuning and ensemble weight optimisation.
- Graph-based feature engineering using transaction relationships.
- Chronological stacking with time-aware validation.
- Pseudo-labelling to incorporate additional information from unlabelled transactions.
- Alternative feature engineering and model combinations.
- Spearman rank-based blending of selected model predictions.

### Temporal Validation

Rather than relying solely on random train-validation splits, models are evaluated using chronological validation periods.

This better reflects the competition's train-test structure, where predictions must generalise to transactions occurring at later time steps.

ROC-AUC is the official competition metric. PR-AUC is also used as a supplementary metric during model development, particularly because of class imbalance.

## Final Models and Results

Two submissions were selected based on their public leaderboard performance.

| Submission | Method | Public ROC-AUC |
|---|---|---|
| `experiment18_stacking.csv` | Chronological stacking | 0.94914 |
| `blend_exp12_18_25.csv` | Spearman rank blending | 0.94813 |

The Spearman blend combines predictions from three experiments:

- Experiment 12: Temporal ensemble (notebook 04)
- Experiment 18: Chronological stacking (notebook 06)
- Experiment 25: Pseudo-labelled stacking (notebook 07)

These results are public leaderboard scores and may differ from performance on the private test partition.

## Reproducing the Final Submissions

### Environment Setup

Python 3.12 is used for the final notebook.

Clone the repository:

```bash
git clone https://github.com/charllene/bt4012-fraud-detection.git
cd bt4012-fraud-detection
```

Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

### Dataset Setup

Download the competition datasets from Kaggle and place them in `data/raw/`:

```text
data/raw/
├── train.csv
├── test.csv
├── txs_edgelist.csv
└── sample_submission.csv
```

The dataset files are excluded from version control.

### Running the Final Notebook

Open:

```text
Final/A0282609X.ipynb
```

Select the project's Python virtual environment as the Jupyter kernel, then execute all cells in order.

The notebook includes the modelling pipelines and submission generation steps for the two selected final models.

The generated files are saved to `final_outputs/`:

```text
final_outputs/
├── experiment18_stacking.csv
└── blend_exp12_18_25.csv
```

### Reproducibility Verification

The final notebook was executed successfully and both generated submission files were compared against the original CSVs submitted to Kaggle.

Both files matched their respective originals numerically, with an absolute tolerance of \(10^{-12}\), and their transaction indices were in the same order.

## Limitations

The primary challenge is temporal distribution shift between the training and test periods. Validation performance may not fully reflect performance on future transactions.

Other limitations include class imbalance, anonymised transaction features that restrict interpretability, and uncertainty introduced by pseudo-labelling.

Public leaderboard results are also based on only a subset of the test data, so the final private leaderboard ranking may differ.

## Project Information

**Module:** BT4012 — Fraud Detection

**Institution:** National University of Singapore

**Academic Year:** 2026/2027 Semester 1

**Student Number:** A0282609X
