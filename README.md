projeto1-qsar-classico


Classical QSAR pipeline: ChEMBL bioactivity data → RDKit molecular fingerprints → scikit-learn (Random Forest, SVM) → cross-validated evaluation.



Part of a personal cheminformatics/ML portfolio developed in parallel with research at LabMol (UFG), aimed at building applied AI skills for drug discovery.


Objective

Build an end-to-end classical QSAR pipeline from scratch:


Select a biological target and download bioactivity data from ChEMBL
Curate and preprocess the dataset (activity labels, duplicates, structural filtering)
Generate molecular fingerprints with RDKit (Morgan/ECFP)
Train and compare ML models: Random Forest and SVM
Evaluate with cross-validation and appropriate metrics (AUC for classification, R² for regression)



Project Structure

projeto1-qsar-classico/
├── data/
│   ├── raw/          # Data as downloaded from ChEMBL (unmodified)
│   └── processed/    # Cleaned, curated, feature-ready datasets
├── notebooks/        # Exploratory and step-by-step analysis notebooks
├── src/              # Reusable Python modules (data loading, featurization, modeling)
├── models/           # Saved trained models
└── results/          # Metrics, plots, evaluation outputs


Stack

ToolPurposePython 3.11Core languageRDKitMolecular fingerprints (Morgan/ECFP), SMILES parsingscikit-learnML models (RF, SVM), cross-validation, metricschembl_webresource_clientChEMBL API data downloadpandas / numpyData manipulationmatplotlib / seabornVisualization

Environment: conda (qsar-proj1), managed via environment.yml.


Target & Dataset


🔲 To be defined — this section will be updated once the ChEMBL target is selected.



Criteria for target selection:


Well-curated bioactivity data in ChEMBL (IC50, Ki)
≥ 500 compounds with measured activity
Biologically relevant to drug discovery context



Pipeline Overview

ChEMBL API
    ↓
Raw bioactivity data (IC50, SMILES)
    ↓
Data curation (remove invalids, duplicates, apply activity threshold)
    ↓
Molecular fingerprints via RDKit (Morgan r=2, 2048 bits)
    ↓
Train/test split + cross-validation
    ↓
Random Forest  ←→  SVM
    ↓
Evaluation: AUC-ROC, precision, recall, R² (if regression)


Progress


 Environment setup (qsar-proj1 conda env, all libs validated)
 Project folder structure created
 ChEMBL target selected
 Data download and exploration
 Data curation pipeline
 Fingerprint generation
 Model training (RF + SVM)
 Cross-validation and evaluation
 Results and conclusions



Setup

bash# Clone the repository
git clone https://github.com/<your-username>/projeto1-qsar-classico.git
cd projeto1-qsar-classico

# Create and activate environment
conda env create -f environment.yml
conda activate qsar-proj1

# Launch Jupyter
jupyter notebook


environment.yml will be added in the next step.




Context

This is Project 1 of a personal AI/cheminformatics portfolio. Projects are designed to build progressively toward production-level skills in computational drug discovery and applied ML.


Project 1 (this): Classical QSAR with fingerprints + scikit-learn
Project 2 (planned): Graph neural networks for molecular property prediction
Project 3 (planned): Generative models for de novo drug design



Author

Giovanna - Universidade Federal de Goiás
Cheminformatics · Machine Learning · Drug Discovery