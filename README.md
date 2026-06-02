# QSAR Modeling for EGFR/ErbB Inhibitors
**Predicting IC₅₀ binding affinity with machine learning**

Built as part of SAAS@Berkeley, Spring 2025.

---

## Overview

This project builds a machine learning pipeline to predict the binding affinity (pIC₅₀) of small molecules targeting the EGFR/ErbB receptor tyrosine kinase family — a well-validated set of cancer drug targets. The core question: given a molecule's chemical structure, can we predict how potently it inhibits the target?

The answer, it turns out, is yes — with R² = 0.82 on held-out data.

---

## Pipeline

**1. Data collection**
Bioactivity data (IC₅₀ values) pulled directly from ChEMBL via the `chembl_webresource_client` API, targeting EGFR, ErbB-3, ErbB-4, and related receptor tyrosine kinases in *Homo sapiens*.

**2. Data cleaning & preprocessing**
- Filtered to IC₅₀ assay type with valid SMILES strings
- Converted IC₅₀ → pIC₅₀ (−log₁₀ transform) for modeling
- Removed duplicates and standardized units

**3. Molecular descriptor engineering**
- Physicochemical and topological descriptors computed via **RDKit**
- Fingerprint-based descriptors computed via **PaDEL**
- Zero-variance and all-zero columns removed
- Resulting feature matrix merged with bioactivity labels

**4. Modeling**
- Train/test split (80/20, random_state=42)
- **Random Forest Regressor** (sklearn) as primary model
- **LazyRegressor** for baseline comparison across multiple algorithms
- Hyperparameter optimization and cross-validation applied

**5. Evaluation**
- R² score and MSE on held-out test set
- Predicted vs. true pIC₅₀ scatter plot with linear regression overlay
- Residual plot for error distribution analysis
- Top 20 feature importances visualized

---

## Results

| Metric | Value |
|---|---|
| R² (test set) | 0.82 |
| MSE reduction vs. baseline | 18% |

---

## Stack

- Python 3.11
- `chembl_webresource_client` — bioactivity data
- `RDKit` — molecular descriptor computation
- `PaDEL` — fingerprint descriptors
- `scikit-learn` — modeling and evaluation
- `pandas`, `numpy` — data processing
- `matplotlib` — visualization
- Google Colab (T4 GPU)

---

## How to run

1. Open the notebook in Google Colab
2. Mount your Google Drive when prompted
3. Run all cells in order — data is fetched live from ChEMBL

---

## Background

EGFR and the ErbB family are among the most studied oncology targets in existence. Drugs like erlotinib, gefitinib, and lapatinib all target this family. QSAR modeling — predicting biological activity from chemical structure — is a foundational technique in computational drug discovery, and this project applies it end-to-end from raw database queries to a validated predictive model.
