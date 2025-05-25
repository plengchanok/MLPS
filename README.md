# Predicting Vaccination Uptake and COVID-19 Cases Using Machine Learning

## Project Overview

This project aims to develop predictive models for two critical COVID-19 outcomes:
- **Task 1:** Predicting county-level vaccination rates.
- **Task 2:** Predicting county-level COVID-19 positive test rates.

These predictions are valuable for informing public health policy, optimizing resource allocation, and improving preparedness for future pandemics.

The analysis leverages data from the **COVID-19 Trends and Impact Survey (CTIS)**, conducted by Carnegie Mellon University in collaboration with Facebook. The dataset contains daily county-level survey responses from U.S. adults in early 2021 and includes features such as behavior, beliefs, and health indicators.

---

## Dataset

- **Source:** COVID-19 Trends and Impact Survey (CTIS)
- **Size:** 25,626 entries from Jan 7 to Feb 12, 2021
- **Features:** 19 behavioral, belief, and health-related variables
- **Targets:**
  - `smoothed_wcovid_vaccinated`: % of adults vaccinated (smoothed)
  - `smoothed_wtested_positive_14d`: 14-day test positivity rate

---

## Task 1: Predicting Vaccination Rates

### Data Preprocessing
- Forward-fill imputation for 10% missing vaccination data
- Feature engineering: rolling averages, lagged variables, behavioral interaction terms
- Temporal train/validation/test split to avoid data leakage

### Models
- **Baseline:** Elastic Net
- **Advanced Models:**  
  - Fixed Effects Panel Regression  
  - Regression with Spatial-Temporal Features  
  - Random Forest  
  - Fully Connected Neural Network  
  - LightGBM (discarded due to overfitting)

### Best Model: Panel Regression
- Lowest test RMSE: **1.63**
- Captured spatial-temporal dynamics and controlled for county-specific effects
- Key predictors:
  - Lagged vaccination uptake
  - Community behavior patterns
  - Temporal momentum (e.g., 3-day rolling average)

---

## Task 2: Predicting COVID-19 Positive Cases

### Data Preprocessing
- High missingness (84%) addressed via moving average imputation
- Feature engineering: lagged features, spatial adjacency matrix (same-state counties)
- Used 80/10/10 temporal splits for training, validation, and testing

### Models
- **Baseline:** Ridge Regression
- **Advanced Models:**
  - Multi-Layer Perceptron (MLP)
  - Spatial Lag Regression (SLR)
  - Random Forest (best performing)

### Best Model: Random Forest (w/ moving average imputation)
- R² = **0.998**, RMSE = **0.09**
- Outperformed deep models and spatio-linear models
- Top features:
  - Vaccination rates (current and lagged)
  - COVID-like illness (CLI) indicators
  - Behavioral activity (e.g., time outside)

---

## Policy Recommendations

- **Vaccination Campaigns:** Leverage social momentum. Prioritize regions with low uptake using tailored messaging (e.g., autonomy-focused for risk-tolerant groups).
- **Early Warning Systems:** Use lagged behavioral and CLI features as surveillance signals.
- **Regional Coordination:** Shared policies between adjacent counties improve intervention efficiency.

---

## Limitations

- High missingness and imputation may introduce bias
- County-level aggregation masks intra-county behavior
- Spatial model assumes equal influence across all counties in the same state
- Deep learning (MLP) underperformed; LSTM or Transformer models suggested for future work

---

## Authors

- Jan Park (jaeunp)  
- Sungmyeon Park (sungmyep)  
- Pleng Witayaweerasak (pwitayaw)  
- Pongsakorn Supakpanichkul (psupak)

---

## Future Directions

- Incorporate real-time data streams
- Apply advanced temporal models (e.g., LSTM, Transformer)
- Refine spatial modeling beyond same-state adjacency
