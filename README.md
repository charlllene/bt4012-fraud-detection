# BT4012 Fraudulent Bitcoin Transaction Detection

This repository contains my individual submission for the BT4012 In-class
Kaggle Competition 2026.

## Objective

The objective is to predict whether Bitcoin transactions are illicit using
anonymised transaction features and graph connections between transactions.

## Repository Structure

- `notebooks/`: exploratory analysis and model development
- `data/raw/`: Kaggle competition data, excluded from GitHub
- `submissions/`: generated Kaggle submissions, excluded from GitHub
- `reports/`: final competition report

## Data

The competition dataset is not included in this repository. Download the
following files from the Kaggle competition and place them in `data/raw/`:

- `train.csv`
- `test.csv`
- `txs_edgelist.csv`
- `sample_submission.csv`

## Evaluation

Models are evaluated using ROC-AUC between the predicted probabilities and
the actual transaction labels.