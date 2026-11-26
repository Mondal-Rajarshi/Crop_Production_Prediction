# Cultivated-Area Dependency in Machine Learning-Based Crop Production Prediction

A machine learning study exploring how much cultivated area contributes to crop production prediction in Bangladesh, and how prediction performance changes when area information is unavailable.

## Overview

Crop production depends on several factors, including cultivated land area, crop type, climate, location, and cultivation practices. However, when predicting total production, cultivated area may become the most influential predictor because production is naturally related to the amount of land being cultivated.

This project investigates that dependency using the **SPAS-Dataset-BD** dataset. Instead of only comparing machine learning algorithms, it evaluates different groups of agricultural features to understand which information contributes most to crop production prediction.

Three regression algorithms are compared:

- Random Forest
- XGBoost
- LightGBM

The study also uses feature importance, permutation importance, and SHAP to interpret model behaviour.

## Research Objectives

The main objectives are to:

- Predict crop production using agricultural and environmental information.
- Compare the performance of three machine learning regression algorithms.
- Evaluate the contribution of different agricultural feature groups.
- Measure how prediction performance changes when cultivated area is excluded.
- Identify influential features using explainability techniques.
- Examine whether the observed performance patterns remain consistent across different validation strategies.

## Dataset

This project uses **SPAS-Dataset-BD (Version 2)**, a Bangladesh-focused dataset for smart precision agriculture.

The original CSV contains:

| Property | Description |
|---|---|
| Total observations | 4,608 |
| Total columns | 15 |
| Crop types | 73 |
| Districts | 64 |
| Target variable | Production |

The dataset includes information about cultivated area, crop names, districts, seasons, temperature, humidity, transplanting periods, growth periods, and harvesting periods.

**Dataset source:** [SPAS-Dataset-BD – Mendeley Data](https://data.mendeley.com/datasets/cphdw4z5kw/2)

## Data Preprocessing

Before model evaluation, the dataset was examined for missing values, invalid records, duplicate rows, and possible target leakage.

The following steps were performed:

1. Removed 416 observations with zero cultivated area.
2. Removed one observation with a missing Season value.
3. Excluded the `AP Ratio` column because it is derived from Production and Area and could introduce target leakage.
4. Applied one-hot encoding to categorical variables.
5. Kept numerical variables in their original numerical form.

After preprocessing, the final dataset contained **4,191 observations and 14 columns**, including the target variable.

## Feature Groups and Experiments

To understand the contribution of different information sources, five feature configurations were evaluated.

| Experiment | Included features |
|---|---|
| Area Only | Cultivated area |
| Agroclimatic Only | Temperature and humidity variables |
| Crop + Lifecycle | Crop name, season, transplant, growth, and harvest |
| Without Area | Agroclimatic, crop, district, and lifecycle features |
| Full Features | All valid predictors except AP Ratio |

This design makes it possible to compare prediction performance with and without cultivated-area information.

## Machine Learning Algorithms

Three tree-based ensemble regression algorithms were used.

**1. Random Forest**

Combines multiple decision trees to produce predictions and capture nonlinear relationships.

**2. XGBoost**

Uses gradient boosting to build an efficient and powerful ensemble of decision trees.

**3. LightGBM**

Uses a gradient-boosting decision-tree approach designed for efficient model training.

Each model was configured with 200 estimators and `random_state=42`. The same general model settings were used across the feature configurations.

## Model Evaluation

The project uses three regression evaluation metrics:

- **Mean Absolute Error (MAE):** Measures the average absolute difference between actual and predicted production.
- **Root Mean Squared Error (RMSE):** Measures prediction error while giving larger errors more weight.
- **R² Score:** Measures how well the model explains variation in the target variable.

Two validation strategies were used:

- **Five-fold shuffled cross-validation:** Evaluates performance across five shuffled data folds.
- **District-based GroupKFold:** Evaluates geographic generalization by keeping observations from the same district in the same fold.

A separate 80/20 holdout split was used for the model interpretation experiments.

## Explainability Analysis

To better understand how the trained models make predictions, three interpretation techniques were applied.

- **Feature Importance:** Identifies influential encoded features in the Random Forest model.
- **Permutation Importance:** Measures the change in model performance when individual input features are shuffled.
- **SHAP:** Explains how individual features contribute to model predictions.

A separate Random Forest model was also trained without cultivated area to examine which features become more influential when Area is unavailable.

## Key Results

### Five-Fold Cross-Validation

The following table shows the mean R² scores across five folds.

| Feature Configuration | Random Forest | XGBoost | LightGBM |
|---|---:|---:|---:|
| Area Only | 0.700 | **0.782** | 0.773 |
| Agroclimatic Only | 0.394 | 0.394 | 0.394 |
| Crop + Lifecycle | 0.509 | 0.509 | 0.509 |
| Without Area | 0.437 | 0.486 | **0.510** |
| Full Features | **0.916** | 0.899 | 0.889 |

### Main Observations

- The Full Features configuration achieved the strongest overall performance with Random Forest, obtaining an R² score of approximately **0.916**.
- Area Only achieved an R² score of approximately **0.782** with XGBoost.
- Without Area, LightGBM achieved the highest R² score of approximately **0.510**.
- Removing cultivated area caused a considerable reduction in predictive performance.
- Feature importance, permutation importance, and SHAP consistently identified Area as the dominant predictor in the Full Features Random Forest model.

District-based GroupKFold validation showed a similar overall pattern. The best Full Features result was approximately 0.919, while the best Without Area result was approximately 0.513.

These findings indicate that cultivated area provides substantial predictive information for total crop production, while crop, geographic, agroclimatic, and lifecycle features provide additional but weaker information.

## Technologies and Libraries

The project was developed using Python and the following libraries:

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- LightGBM
- SHAP
- Google Colab

## How to Run the Project

The notebook is designed to run in Google Colab.

1. Download or clone this repository.
2. Open `datamining 2.1.ipynb` in Google Colab.
3. Upload the `SPAS-Dataset-BD.csv` dataset when prompted.
4. Run the notebook cells in order.
5. Review the model evaluation tables, plots, and explainability results.

If a required library is missing in your environment, install it before running the relevant cells.

## Project Structure

```text
Crop-Production-Prediction/
│
├── datamining 2.1.ipynb
├── SPAS-Dataset-BD.csv
└── README.md
```

The dataset file should be placed in the project directory or uploaded when the notebook requests it.

## Limitations and Future Work

This study uses a single Bangladesh-specific agricultural dataset. The available features do not include some potentially useful information, such as rainfall, soil properties, irrigation, and remote-sensing indicators.

The models also use fixed estimator settings rather than extensive hyperparameter optimization.

Future work could explore additional agricultural datasets, richer environmental information, and crop yield prediction per unit of cultivated area. This would help examine whether the observed dependence on cultivated area remains when the prediction target is no longer total production.

## Conclusion

This project demonstrates that cultivated area is a major source of predictive information when estimating total crop production using the SPAS-Dataset-BD dataset.

By comparing five feature configurations, three regression algorithms, two validation strategies, and multiple explainability techniques, the study provides a clearer understanding of how agricultural information influences crop production prediction.

The main focus is not simply identifying the best-performing algorithm, but understanding **how much the prediction depends on cultivated-area information and what predictive capability remains without it**.

---

**Research focus:** Machine Learning | Agricultural Data Analysis | Crop Production Prediction | Feature Dependency | Explainable AI
