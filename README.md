# AI4Food: Metabolic Phenotyping Using Multimodal Health Data

## Overview
This repository contains the code and analysis pipeline developed during a 6-month master's thesis internship at IMDEA Nutrition (Madrid, Spain), Computational Biology Group, supervised by Dr. Enrique Carrillo de Santa Pau, as part of the AI4Food project. The project integrates multimodal longitudinal health data to characterize metabolic phenotypes in overweight and obese adults using machine learning and explainable AI.

**Preprint:** Attioui H., Lacruz-Pleguezuelos B., Carrillo de Santa Pau E. (2026). Multi-Modal Wearable and Biological Data Integration Reveals Physiological Signatures of Weight Variation in Overweight and Obese Adults: An AI4Food Machine Learning Study. medRxiv. doi: 10.64898/2026.11.03.26367178

## Research questions
1. Can we identify distinct metabolic profiles in individuals with obesity by integrating wearable signals, continuous glucose monitoring, sleep data, nutritional intake and gut microbiome data?
2. Can sleep-derived physiological features predict metabolic phenotypes such as glucose regulation and thermoregulation in previously unseen participants?

## Study design
- 93 participants with overweight or obesity (BMI 27–35 kg/m²)
- 30-day crossover intervention with two monitoring phases:
  - Traditional phase: questionnaires and food diaries
  - Digital phase: Fitbit Sense smartwatch and FreeStyle Libre 2 continuous glucose monitor
  - Group 1: traditional then digital; Group 2: digital then traditional
- Weight measured at three clinical visits (V0, V2, V3)

## Data modalities
- Wearable physiological signals: HRV, SpO2, respiratory rate, skin temperature, sleep stages
- Continuous glucose monitoring every 15 minutes
- Sleep recordings and subjective sleep quality (Oviedo Sleep Questionnaire, OSQ)
- Nutritional intake records (analyzed with DIAL software)
- Gut microbiome (shotgun metagenomic sequencing)

## Data access
The AI4FoodDB dataset is publicly available for research purposes:
- Full dataset: https://bidalab.eps.uam.es/static/AI4FoodDB/AI4FoodDB.zip
- Nutrition dataset (DS3): https://bidalab.eps.uam.es/static/AI4FoodDB/FoodNExtDB.zip
- Gut microbiome (DS5): https://www.ebi.ac.uk/ena/browser/view/PRJEB87701

The database is intended for research purposes only and may not be used in commercial applications.

## Methods
- Multimodal preprocessing and temporal alignment by intervention day (Python)
- K-means clustering (elbow method) to define weight-response, glucose, temperature, respiratory-rate and nutrition profiles
- PCA for nutritional data
- Associations between profiles: chi-square tests on contingency tables, Spearman correlations
- Weight and OSQ comparisons between phases and groups (Wilcoxon tests)
- Random Forest and multilayer perceptron (MLP) classifiers predicting thermoregulation and glucose phenotypes from sleep-derived variables
  - Participant-level 5-fold cross-validation (StratifiedGroupKFold): all nights of a participant stay in the same fold
  - Temperature variables excluded from the thermoregulation model to avoid target leakage
  - MLP inputs standardized within each training fold only
- SHAP on out-of-fold Random Forest predictions

## Key results
- Over the full programme, participants lost about 2 kg on average (p < 0.001).
- In Group 2, weight loss was greater during the digital phase than the traditional phase (−1.51 ± 1.08 vs −0.79 ± 1.11 kg, p = 0.0004). Group 1 showed no significant difference (p = 0.42), and between-group differences were not significant. Because Group 2 started with the digital phase and there was no washout, phase order may explain part of this difference.
- Three weight-response clusters were identified with significantly different outcomes (p < 0.0001).
- Respiratory-rate clusters (p = 0.0014) and skin-temperature clusters (p = 0.0116) were associated with weight-response clusters; nutrition clusters were not (p = 0.57).
- Random Forest showed moderate, above-chance prediction of glucose phenotypes (ROC-AUC 0.699 ± 0.170) and thermoregulation phenotypes (ROC-AUC 0.636 ± 0.034). The MLP performed worse in both tasks, as expected for small tabular datasets.
- SHAP identified RMSSD (HRV), restlessness, REM respiratory-rate variability, breathing rate and NREM heart rate as the most influential predictors.
- Gut microbiome composition remained stable across the intervention and was not associated with weight-response clusters.

## Limitations
- Small effective sample size for predictive modelling; performance varies between folds.
- Phenotype labels were defined by clustering before cross-validation, so the pipeline is not fully nested. A future version should fit the clustering within each training fold.
- Short intervention, no washout period between phases, and missing wearable nights.
- P-values were not corrected for multiple comparisons. All findings are hypothesis-generating.

## Citation
If you use the data, please cite the original publication:
Romero-Tapiador S., et al. AI4FoodDB: A Database for Personalized e-Health Nutrition and Lifestyle through Wearable Devices and Artificial Intelligence. Database: The Journal of Biological Databases and Curation, 2023. DOI: 10.1093/database/baad049

## Tools
Python 3.11 | scikit-learn | SHAP | pandas | NumPy | Matplotlib | Seaborn | JupyterLab

## License
MIT License.
