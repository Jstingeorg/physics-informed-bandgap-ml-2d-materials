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
- Final curated dataset contains ~400 materials

Processed dataset: data/tmdc_data_final.csv


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

### Physics-Informed Advantage
- Model A consistently outperforms Model B across all algorithms
- Inclusion of non-SOC bandgap reduces RMSE by **~30–50%**
- Confirms that physics-informed features significantly improve prediction accuracy

### Best Model Selection
- **GBDT Model A** achieved:
  - Lowest RMSE
  - Best stability across folds
- Selected as the **primary model** for all subsequent analyses

---

## Advanced Analyses (Model A Only)

### 1. Uncertainty Quantification
- Ensemble-based uncertainty estimation using multiple GBDT realizations
- Provides confidence estimates without Bayesian modeling
- Lower uncertainty observed in TMDC-relevant bandgap regions

### 2. Error Analysis by Bandgap Range
- Errors evaluated across discrete bandgap bins
- Lowest error in the **1.2 – 2.0 eV** range
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

## Repository Structure

---

## How to Run
### Requirements
- Python ≥ 3.9
- numpy
- pandas
- scikit-learn
- xgboost
- matplotlib
- joblib

Install dependencies:
```bash
pip install numpy pandas scikit-learn xgboost matplotlib joblib



