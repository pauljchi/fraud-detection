# fraud-detection

## Dataset

The dataset contains credit card transactions made by European cardholders over approximately two days in September 2013.

- 284,807 original transactions
- 492 originally labeled fraud cases
- 28 anonymized PCA-transformed features: `V1`–`V28`
- `Time`: seconds elapsed since the first recorded transaction
- `Amount`: transaction amount
- `Class`: fraud label, where `1` indicates fraud

Dataset source: [Fraud Detection on Kaggle](https://www.kaggle.com/datasets/whenamancodes/fraud-detection)

## Part 1: Data Preparation and Exploratory Analysis

Part 1 is available in ['01_data_preparation_eda.ipynb'](01_data_preparation_eda.ipynb).

The notebook covers:

- Dataset schema and integrity validation
- Missing and non-finite value checks
- Invalid time and amount checks
- Exact duplicate identification and removal
- Class-imbalance analysis
- Transaction amount analysis
- Transaction activity and fraud prevalence over time
- Univariate comparison of the anonymized PCA features
- Predictor correlation analysis
- Preliminary distribution-drift analysis

## Preliminary findings

- The source data contains no missing, infinite, negative-time, or negative-amount values.
- There are 1,081 exact duplicate records beyond their first occurrences.
- After duplicate removal, 283,726 transactions remain.
- The cleaned data contains 473 fraud cases.
- Fraud represents approximately 0.167% of the cleaned data, confirming severe class imbalance.
- Several anonymized PCA components show substantial univariate separation between fraudulent and legitimate transactions.
- Transaction volume, fraud prevalence, and some predictor distributions vary over time.
- Statistical outliers were retained because unusual observations may contain important fraud signals.

## Part 2: Model Development and Evaluation

Part 2 is available in [`02_model_development_evaluation.ipynb`](02_model_development_evaluation.ipynb).

The notebook covers:

- Stratified training and test-set creation
- Comparison of dummy, weighted logistic regression, SMOTE logistic regression, and weighted XGBoost models
- Five-fold stratified cross-validation using Average Precision as the primary metric
- Leakage-safe resampling within cross-validation folds
- Classification-threshold selection using out-of-fold predictions
- Final evaluation on an untouched test set
- Precision-recall and confusion-matrix analysis
- Error analysis and permutation feature importance
- Modeling limitations and deployment considerations

## Model results

- Weighted XGBoost achieved the strongest cross-validation performance, with mean Average Precision of 0.8430.
- The classification threshold was selected entirely from out-of-fold training predictions by maximizing F1.
- On the untouched test set, the final model achieved:
  - 0.8118 Average Precision
  - 0.9103 precision
  - 0.7474 recall
  - 0.8208 F1-score
- The model detected 71 of the 95 fraudulent test transactions, missed 24, and incorrectly flagged 7 legitimate transactions.
- `V4`, `V14`, `V11`, and `V12` had the largest permutation importance values.
- Overall, the project shows that supervised learning can identify fraud in this dataset. However, the model would need more testing over time, probability calibration, a threshold based on real-world costs, and operational validation before it could be used in practice.
