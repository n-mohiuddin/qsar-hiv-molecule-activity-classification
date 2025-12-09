QSAR Modeling for HIV Inhibitor Activity Prediction

A cheminformatics workflow using molecular descriptors and Morgan fingerprints

Project Overview

This project presents a complete QSAR (Quantitative Structure–Activity Relationship) workflow aimed at predicting HIV inhibitor activity using molecular structure data. The study covers preprocessing of SMILES strings, feature engineering, exploratory data analysis, model benchmarking, hyperparameter tuning, and external test evaluation.

The work reflects practical methods used in cheminformatics and early-stage drug discovery, particularly in the context of virtual screening and hit identification.

Dataset Description

The dataset originates from the MoleculeNet HIV benchmark and contains approximately 41,000 molecules labeled according to biochemical assay results:

Active: CA (Confirmed Active), CM (Confirmed Moderately Active)

Inactive: CI (Confirmed Inactive)

The dataset is highly imbalanced, with active compounds representing roughly 4% of all samples. For this reason, evaluation metrics such as PR-AUC and recall are more informative than accuracy.

Domain Insight: Chemical Interpretation

The HIV inhibition assays used to label this dataset reflect the biochemical activity of molecules interacting with viral targets. Physicochemical properties such as molecular weight, lipophilicity (LogP), polar surface area (TPSA), and hydrogen bond donor/acceptor counts influence permeability, solubility, and binding affinity. These descriptors provide interpretable chemical features that form the basis of traditional QSAR approaches.

Morgan fingerprints complement these descriptors by encoding substructural patterns. Models such as Random Forest and XGBoost are well suited for learning complex structure–activity relationships in high-dimensional fingerprint space and are commonly used in early-stage virtual screening to identify promising active compounds under class imbalance.

Project Workflow
1. Data Loading and Cleaning

Loading the HIV dataset from the Hugging Face datasets library

Standardizing and canonicalizing SMILES

RDKit molecule validation

Removal of invalid or unparseable structures

2. Feature Engineering

This step constructs the numerical representation of each molecule.

2.1 Calculation of Physicochemical Descriptors

Computed using RDKit:

Molecular Weight (MolWt)

LogP (MolLogP)

Topological Polar Surface Area (TPSA)

Hydrogen Bond Donors (HBD)

Hydrogen Bond Acceptors (HBA)

Rotatable Bonds

These descriptors serve both interpretability and statistical analysis.

2.2 Morgan Fingerprints

Radius = 2

2048-bit hashed fingerprint

Captures substructural information used for machine learning models

2.3 Hybrid Feature Matrix

Descriptors and fingerprints were concatenated into a unified feature matrix for model development.

3. Exploratory Data Analysis

EDA focused on statistical comparison and visualization of computed descriptors.

Distribution analysis of descriptors for active vs inactive classes

Boxplots and histograms

Mann–Whitney U tests to assess statistically significant differences

Correlation heatmaps to identify redundancy among descriptors

This step provided chemical insight into features associated with activity.

4. Model Development

All models were evaluated using stratified 5-fold cross-validation to ensure fair and consistent comparison.

4.1 Baseline Model — Logistic Regression

Linear decision boundary

Establishes a reference for non-linear models

4.2 Benchmark Model — Random Forest

Captures non-linear structure–activity patterns

Performs well on high-dimensional fingerprint representations

4.3 Advanced Non-Linear Model — XGBoost

Gradient boosting classifier

Improves recall for the minority class in imbalanced classification problems

4.4 Model Comparison

Evaluated metrics:

Accuracy

F1-score (active class)

Recall (active class)

ROC-AUC

PR-AUC (primary metric due to imbalance)

Cross-validated Performance
Model	PR-AUC	F1 (Active)	Recall (Active)
Logistic Regression	0.31	0.28	0.56
Random Forest (Default)	0.48	0.44	0.32
XGBoost	0.45	0.41	0.56
Random Forest (Tuned)	0.42	0.47	0.47
Hyperparameter Tuning

Hyperparameter optimization was attempted using RandomizedSearchCV. Due to instability in PR-AUC across folds and high computational cost, a stable parameter configuration was selected based on domain knowledge:

n_estimators = 300
max_depth = 10
min_samples_split = 5
min_samples_leaf = 1
max_features = 0.5
class_weight = "balanced"


This configuration improved the balance between precision and recall for the minority (active) class.

Final Model Selection

Because this dataset reflects an early-stage virtual screening scenario, identifying active compounds is more important than maximizing overall accuracy. Therefore, PR-AUC and recall were prioritized.

The default Random Forest achieved the highest PR-AUC, indicating strong precision–recall performance.

XGBoost achieved the highest recall, detecting more actives than other models.

Logistic Regression served as a baseline for comparison.

A tuned Random Forest model was ultimately selected because it provided a superior balance between recall and F1-score while maintaining competitive PR-AUC. This trade-off is well suited for virtual screening, where recovering active compounds is a priority.

Test Set Results
Metric	Value
Accuracy	0.9609
F1 (Active)	0.4410
Recall (Active)	0.4394
ROC-AUC	0.8046
PR-AUC	0.3667

Results are consistent with cross-validation, indicating good generalization and no evidence of overfitting.

Model Visualizations

Confusion Matrix

ROC Curve

Precision–Recall Curve

Future Work

Scaffold-based data splitting for improved chemical generalization

Tanimoto similarity clustering

Feature interpretation using SHAP

Applicability Domain estimation

Graph Neural Networks (GNNs) for molecular representation

Author

Nezihe Mohiuddin
Email: nezihemohiuddin@gmail.com
