# projeto1-qsar-classico

> Classical QSAR pipeline: ChEMBL bioactivity data → RDKit molecular fingerprints → scikit-learn (Random Forest, SVM) → cross-validated evaluation, applied to BTK inhibitors.

Part of a personal cheminformatics/ML portfolio developed in parallel with research at **LabMol (UFG)**, aimed at building applied AI skills for drug discovery.

---

## Overview

This project builds a complete classical QSAR pipeline from raw bioactivity
data to trained, evaluated, and tuned machine learning models — applied to
**BTK (Bruton's Tyrosine Kinase)**, a key therapeutic target in autoimmune
diseases (lupus, rheumatoid arthritis) and B-cell malignancies.

The project covers both **classification** (active/inactive) and
**regression** (continuous pIC50 prediction), compares Random Forest against
SVM/SVR throughout, and includes hyperparameter tuning and class-imbalance
analysis — documented with full reasoning at each step.

---

## Target

| Field | Value |
|-------|-------|
| **Target** | Bruton's Tyrosine Kinase (BTK) |
| **ChEMBL ID** | CHEMBL2842 |
| **Organism** | *Homo sapiens* |
| **Activity type** | IC50 |
| **Raw records collected** | 6,502 |
| **Curated unique compounds** | 4,354 |
| **Therapeutic area** | Autoimmune diseases, B-cell malignancies |
| **Reference drugs** | Ibrutinib, Acalabrutinib, Zanubrutinib |

---

## Key Results

### Classification (active vs. inactive, pIC50 ≥ 6.0 threshold)

| Model | Test AUC-ROC | Test F1 |
|-------|---------------|---------|
| **Random Forest** | **0.953** | **0.946** |
| SVM | 0.938 | 0.930 |

### Regression (continuous pIC50 prediction)

| Model | Test R² | Test RMSE |
|-------|---------|-----------|
| **Random Forest** | **0.742** | **0.602** |
| SVR | 0.687 | 0.663 |

**Random Forest outperformed SVM/SVR on every metric, in both tasks**, and
trained significantly faster. See `notebooks/04` and `notebooks/05` for the
full comparison and reasoning.

### Hyperparameter tuning (notebook 06)

GridSearchCV/RandomizedSearchCV found that **default hyperparameters were
already near-optimal** for both models — tuning produced no measurable
improvement in CV AUC. This indicated the performance bottleneck was
elsewhere (data/class balance), not model configuration.

### Class imbalance strategies (notebook 07)

The dataset is imbalanced (82.5% active / 17.5% inactive). Four strategies
were compared on the Random Forest classifier:

| Strategy | AUC | F1 (Inactive) | Recall (Inactive) |
|----------|-----|----------------|----------------------|
| **Baseline** (`class_weight='balanced'`) | 0.953 | **0.765** | 0.822 |
| SMOTE oversampling | 0.947 | 0.749 | 0.816 |
| Random undersampling | 0.938 | 0.682 | 0.868 |
| **Threshold adjustment (0.65)** | 0.953 | 0.744 | **0.882** |

No single strategy dominates — each optimizes a different objective. The
project formalizes **two models as primary outputs**:
1. **Baseline** — best general-purpose balance between classes
2. **Same model + threshold=0.65** — best for conservative virtual screening
   triage (prioritizes catching true inactives), requires no retraining

SMOTE and undersampling underperformed the baseline and are documented as
comparison evidence only — not carried into `src/`.

### Chemical interpretability: bit_56

Across **both** the classification and regression Random Forest models, the
single most important fingerprint feature is consistently **bit_56**,
decoded via RDKit as a **urea/amide-centered substructure**
(–NH–C(=O)–NH–) — a known hydrogen-bond donor/acceptor motif common in
kinase inhibitors that interacts with the ATP-binding hinge region. This
convergence across two independently trained models on two different tasks
is strong evidence the models learned a chemically meaningful
structure-activity relationship, not statistical noise.

---

## Pipeline

```
ChEMBL API (CHEMBL2842)
        ↓
01_data_collection.ipynb / src/data_collection.py
  └─ Raw IC50 data + SMILES → data/raw/btk_raw.csv (6,502 records)
        ↓
02_preprocessing.ipynb / src/preprocessing.py
  └─ Curation + IC50 → pIC50 + activity labels → data/processed/btk_curated.csv (4,354 molecules)
        ↓
03_fingerprints.ipynb / src/featurization.py
  └─ Morgan Fingerprints (ECFP4, r=2, 2048 bits) → data/processed/btk_fps.csv
        ↓
04_modeling.ipynb / src/train_classifier.py
  └─ RF vs SVM classification → AUC 0.953 vs 0.938
        ↓
05_regression.ipynb / src/train_regressor.py
  └─ RF vs SVR regression → R² 0.742 vs 0.687
        ↓
06_tuning_classification.ipynb
  └─ GridSearchCV/RandomizedSearchCV → defaults confirmed near-optimal
        ↓
07_imbalance_strategies.ipynb
  └─ SMOTE / undersampling / threshold tuning → baseline + threshold=0.65 formalized
```

---

## Project Structure

```
projeto1-qsar-classico/
├── data/
│   ├── raw/                          # ChEMBL raw download (gitignored)
│   └── processed/                    # Curated + featurized datasets (gitignored)
├── notebooks/                        # Full narrative pipeline with analysis
│   ├── 01_data_collection.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_fingerprints.ipynb
│   ├── 04_modeling.ipynb
│   ├── 05_regression.ipynb
│   ├── 06_tuning_classification.ipynb
│   └── 07_imbalance_strategies.ipynb
├── src/                               # Production-ready reusable scripts
│   ├── __init__.py
│   ├── utils.py                       # Shared helpers (fingerprinting, pIC50 conversion)
│   ├── data_collection.py
│   ├── preprocessing.py
│   ├── featurization.py
│   ├── train_classifier.py
│   ├── train_regressor.py
│   └── main.py                        # Orchestrates the full pipeline end-to-end
├── models/                            # Saved trained models (gitignored)
├── results/                           # Plots and evaluation outputs (gitignored)
├── environment.yml
└── README.md
```

---

## Stack

| Tool | Purpose |
|------|---------|
| Python 3.11 | Core language |
| RDKit | Morgan fingerprints (ECFP4), SMILES parsing/validation |
| scikit-learn | RF, SVM/SVR, cross-validation, GridSearchCV, metrics |
| imbalanced-learn | SMOTE, random undersampling |
| chembl_webresource_client | ChEMBL API data download |
| pandas / numpy | Data manipulation |
| matplotlib / seaborn | Visualization |

**Environment**: conda (`qsar-proj1`), see `environment.yml`.

---

## Setup

```bash
git clone https://github.com/GiovannaBorgesBasso/qsar-classico.git
cd qsar-classico

conda env create -f environment.yml
conda activate qsar-proj1

python -m ipykernel install --user --name qsar-proj1 --display-name "Python 3 (qsar-proj1)"
```

### Run the full pipeline

```bash
# From scratch (re-downloads from ChEMBL, ~7 min)
python -m src.main

# Reuse existing raw data (skips download)
python -m src.main --skip-download
```

### Or run individual steps

```bash
python -m src.data_collection
python -m src.preprocessing
python -m src.featurization
python -m src.train_classifier
python -m src.train_regressor
```

### Or explore interactively

```bash
jupyter notebook
# open notebooks/ in order, 01 through 07
```

---

## Limitations

- **2D fingerprints only** — ECFP4 encodes substructure topology, not 3D
  conformation or binding geometry.
- **Class imbalance partially addressed, not eliminated** — inactive-class
  performance (F1 ≈ 0.74–0.77) remains below active-class performance
  (F1 ≈ 0.93–0.95) across all strategies tested.
- **Extrapolation limits** — both RF and SVR compress predictions toward
  the mean at extreme pIC50 values (very weak or very potent compounds),
  a known limitation of tree-based and kernel models on out-of-distribution data.
- **No applicability domain analysis** — predictions for molecules very
  structurally different from the training set should be treated with caution.

---

## Context

This is **Project 1** of a personal AI/cheminformatics portfolio, building
progressively toward production-level skills in computational drug discovery.

- **Project 1** (this): Classical QSAR — fingerprints + scikit-learn
- Project 2 (planned): Graph neural networks for molecular property prediction
- Project 3 (planned): Generative models for de novo drug design

---

## Author

Giovanna — LabMol, Universidade Federal de Goiás
*Cheminformatics · Machine Learning · Drug Discovery*