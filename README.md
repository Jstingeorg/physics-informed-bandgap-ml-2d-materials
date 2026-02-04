# physics-informed-bandgap-ml-2d-materials

## Overview
This project develops a **physics-informed machine learning framework** for predicting
**spin–orbit coupling (SOC)–corrected electronic bandgaps** of **two-dimensional (2D) materials**,
with a focused application to **transition-metal dichalcogenides (TMDCs)**.

Instead of predicting bandgaps purely from generic material descriptors, the framework
explicitly incorporates **non-SOC DFT bandgaps as a physical prior**, enabling the model to
learn **SOC-induced bandgap corrections** in a physically meaningful and data-efficient way.

---

## Motivation
- SOC effects are significant in TMDCs due to heavy transition metals (Mo, W)
- SOC-inclusive DFT calculations are computationally expensive
- Physics-informed ML provides faster, accurate alternatives
- Enables high-throughput screening of 2D semiconductors

---

## Dataset Preparation
- Initial dataset compiled from publicly available 2D materials databases
- Features include:
  - Density of states at Fermi level (`dosef`)
  - Total energy (`energy`)
  - Atomic mass (`mass`)
  - Formation enthalpy (`hform`)
  - Unit-cell volume (`volume`)
  - **Non-SOC DFT bandgap (`gap_nosoc`)**
- Target property:
  - **SOC-corrected DFT bandgap (`gap`)**

### TMDC-Relevant Bandgap Filtering
To focus on technologically relevant materials:
- Bandgap range restricted to **0.8 – 2.5 eV**
- This range captures most semiconducting TMDC monolayers
- Final curated dataset contains 415 materials

Processed dataset: 'data/tmdc_data_final.csv'


---

## Methodology

### Physics-Informed Model Design
Two feature configurations were constructed:

**Model A (Physics-Informed)**
- All descriptors + `gap_nosoc`

**Model B (Baseline)**
- Same descriptors excluding `gap_nosoc`

This controlled comparison isolates the effect of incorporating physical prior knowledge.

---

### Machine Learning Models
Three tree-based regression models were trained and evaluated:
- Gradient Boosting Decision Trees (GBDT)
- Random Forest (RF)
- XGBoost (XGB)

Model performance was assessed using **5-fold cross-validation**, with
**Root Mean Square Error (RMSE)** as the evaluation metric.

---

## Results

| Model | Model A RMSE (eV) | Model B RMSE (eV) |   Improvement  |
|-------|-------------------|-------------------|----------------|
| GBDT  |    **0.160261**   |      0.351136     | **54.359217%** |
| RF    |      0.189887     |      0.349350     |   45.645632%   |
| XGB   |      0.168604     |      0.347531     |   51.485117%   |

### Physics-Informed Advantage
- Model A consistently outperforms Model B across all algorithms
- Inclusion of non-SOC bandgap reduces RMSE by **~50%**
- Confirms that physics-informed features significantly improve prediction accuracy

### Best Model Selection
- **GBDT Model A** achieved the best overall performance:
  - Lowest RMSE: **0.16 eV**
  - Smallest cross-validation variance
- Selected as the **primary model** for all subsequent analyses

---

## Advanced Analyses (Model A Only)

### 1. Uncertainty Quantification
- Ensemble-based uncertainty estimation using multiple GBDT realizations
- Mean predictive uncertainty: **0.0053 eV**
- Median predictive uncertainty: **0.0051 eV**
- Lower uncertainty observed in TMDC-relevant bandgap regions

### 2. Error Analysis by Bandgap Range
- Errors evaluated across discrete bandgap bins
- Lowest error in the **2.0 – 2.5 eV** range
- Increased error near bandgap boundaries due to reduced data density

### 3. Feature Importance Consistency
- Feature importance compared across GBDT, RF, and XGB
- `gap_nosoc` consistently identified as the dominant feature
- Confirms physical interpretability and model robustness

### 4. Parity Plot
- Predicted vs DFT bandgap comparison
- Demonstrates strong agreement with minimal bias

### 5. Learning Curve Analysis
- Shows stable convergence with increasing data
- Indicates absence of overfitting and potential benefit from larger datasets

---

---

## Requirements
- Python ≥ 3.9
- numpy, pandas
- scikit-learn
- xgboost
- matplotlib
- joblib

---

## References
1. Jain et al., *PLOS ONE*, 2021  
   



