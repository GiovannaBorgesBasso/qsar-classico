# projeto1-qsar-classico

> Classical QSAR pipeline: ChEMBL bioactivity data → RDKit molecular fingerprints → scikit-learn (Random Forest, SVM) → cross-validated evaluation.

Part of a personal cheminformatics/ML portfolio developed in parallel with research at **LabMol (UFG)**, aimed at building applied AI skills for drug discovery.

---

## Objective

Build an end-to-end classical QSAR pipeline from scratch, applied to **BTK (Bruton's Tyrosine Kinase)** inhibitors — a key therapeutic target in autoimmune diseases (lupus, rheumatoid arthritis) and B-cell malignancies.

1. Download bioactivity data from ChEMBL (IC50, human BTK)
2. Curate and preprocess the dataset (activity labels, duplicates, structural filtering)
3. Generate molecular fingerprints with RDKit (Morgan/ECFP4)
4. Train and compare ML models: Random Forest and SVM
5. Evaluate with cross-validation and appropriate metrics (AUC-ROC, F1)

---

## Target

| Field | Value |
|-------|-------|
| **Target** | Bruton's Tyrosine Kinase (BTK) |
| **ChEMBL ID** | CHEMBL2842 |
| **Organism** | *Homo sapiens* |
| **Activity type** | IC50 |
| **Compounds (raw)** | ~4,400 |
| **Therapeutic area** | Autoimmune diseases, B-cell malignancies |
| **Reference drugs** | Ibrutinib, Acalabrutinib, Zanubrutinib |

BTK is a non-receptor tyrosine kinase central to B-cell receptor (BCR) signaling. Its overexpression drives hyperactivation of B-cells, contributing to autoimmune conditions such as Systemic Lupus Erythematosus (SLE) and Rheumatoid Arthritis (RA), as well as B-cell lymphomas.

---

## Pipeline

```
ChEMBL API (CHEMBL2842)
        ↓
01_data_collection.ipynb
  └─ Raw IC50 data + SMILES → data/raw/btk_raw.csv
        ↓
02_preprocessing.ipynb
  └─ Curation + IC50 → pIC50 + activity labels → data/processed/btk_curated.csv
        ↓
03_fingerprints.ipynb
  └─ Morgan Fingerprints (ECFP4, r=2, 2048 bits) via RDKit → data/processed/btk_fps.csv
        ↓
04_modeling.ipynb
  └─ Random Forest + SVM → 5-fold cross-validation → models/
        ↓
05_evaluation.ipynb
  └─ AUC-ROC, F1, confusion matrix, feature importance → results/
```

---

## Project Structure

```
projeto1-qsar-classico/
├── data/
│   ├── raw/             # Data as downloaded from ChEMBL (unmodified)
│   └── processed/       # Cleaned, curated, feature-ready datasets
├── notebooks/           # Step-by-step analysis notebooks (01 → 05)
├── src/                 # Reusable Python modules
├── models/              # Saved trained models (.pkl)
└── results/             # Metrics, plots, evaluation outputs
```

---

## Stack

| Tool | Purpose |
|------|---------|
| Python 3.11 | Core language |
| RDKit | Morgan fingerprints (ECFP4), SMILES parsing/validation |
| scikit-learn | RF, SVM, cross-validation, metrics |
| chembl_webresource_client | ChEMBL API data download |
| pandas / numpy | Data manipulation |
| matplotlib / seaborn | Visualization |

**Environment**: conda (`qsar-proj1`), see `environment.yml`.

---

## Progress

- [x] Environment setup (`qsar-proj1` conda env validated)
- [x] Project folder structure created
- [x] Repository initialized with README and `.gitignore`
- [x] Target selected: BTK (CHEMBL2842)
- [ ] Data download and exploration (`01_data_collection.ipynb`)
- [ ] Data curation and pIC50 conversion (`02_preprocessing.ipynb`)
- [ ] Fingerprint generation and EDA (`03_fingerprints.ipynb`)
- [ ] Model training — RF + SVM (`04_modeling.ipynb`)
- [ ] Evaluation and comparison (`05_evaluation.ipynb`)

---

## Setup

```bash
# Clone the repository
git clone https://github.com/GiovannaBorgesBasso/qsar-classico.git
cd qsar-classico

# Create and activate environment
conda env create -f environment.yml
conda activate qsar-proj1

# Launch Jupyter
jupyter notebook
```

---

## Context

This is **Project 1** of a personal AI/cheminformatics portfolio, building progressively toward production-level skills in computational drug discovery.

- **Project 1** (this): Classical QSAR — fingerprints + scikit-learn
- Project 2 (planned): Graph neural networks for molecular property prediction
- Project 3 (planned): Generative models for de novo drug design

---

## Author

Giovanna — LabMol, Universidade Federal de Goiás  
*Cheminformatics · Machine Learning · Drug Discovery*