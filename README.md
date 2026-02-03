# 🛡️ Insurance Claims Ultimate Cost Prediction

This project focuses on inferring missing values for the *Coût Ultime* (Ultimate Cost) variable in a 1988-2006 insurance panel dataset. Developed as part of the *Apprentissage Supervisé* course at **ISFA** (Institut de Science Financière et d'Assurances).

## 🎯 Objective

To predict and impute missing *Coût Ultime* values using advanced Machine Learning techniques, prioritizing model robustness against outliers.

## ⚙️ Methodology

* **Workflow:** Extensive EDA, Feature Engineering, and model benchmarking (Regression, Classification, Boosting).
* **Model Selection:** **CatBoost** was selected as the best performer (lowest RMSE/MAE, highest $R^2$) despite higher inference time.
* **Optimization:** Hyperparameter tuning via **Optuna** using **Huber Loss** to handle significant outliers in claim costs.

## 🔑 Key Features

The model relies heavily on the following predictors:

1. Logarithmic *Coût Initial*
2. Topic Scores (NLP)
3. Effective Hourly Wage
4. Time-to-Communication & Claim-to-Salary ratios

## 📊 Results

The imputed *Coût Ultime* distribution effectively mirrors the statistical properties of the initial cost (size-adjusted), validating the model's consistency on out-of-sample data.
