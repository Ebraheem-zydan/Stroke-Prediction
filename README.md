# Stroke Risk Prediction

Predicting stroke risk from patient health and lifestyle data with classical ML and ensemble models.
This was a team project. I owned the **modeling**: preprocessing pipeline, model selection, hyperparameter tuning and evaluation.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-EC6B23)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)

## Dataset
[`Dataset.csv`](Dataset.csv): 5,110 patients, 11 features (age, gender, hypertension, heart disease, marital status, work type, residence, average glucose, BMI, smoking status) and a binary `stroke` target.
The data is **heavily imbalanced**: only about 5% of patients had a stroke.

## Pipeline
1. **EDA**: 15 guided questions (e.g. *does hypertension raise stroke risk?*, *BMI and glucose vs. stroke*) answered with seaborn/plotly charts.
2. **Cleaning**: null handling (BMI), outlier treatment, duplicate checks.
3. **Encoding and scaling**: categorical encoding + `StandardScaler`.
4. **Class balancing**: upsampling of the minority class with `sklearn.utils.resample`.
5. **Modeling**: six model families tuned with `GridSearchCV`.

## Results (test set, n = 1,931)

| Model | Accuracy | Notes |
|---|---|---|
| **Stacking** (SVM + Decision Tree + LogReg + KNN → LogReg) | **0.992** | best overall |
| Decision Tree (gini, tuned) | 0.981 | |
| XGBoost (lr 0.2, depth 5, 300 trees) | 0.978 | train acc 0.9999 |
| KNN (k=3, manhattan, distance-weighted) | 0.946 | |
| Logistic Regression (C=1, L2) | 0.776 | linear baseline |

## ⚠️ Known limitation, and what I'd do differently
The minority class was upsampled **before** the train/test split, so duplicated stroke cases appear in both sets. This inflates the scores above. The near-perfect train accuracy for XGBoost is a symptom.
The fix:
- Split first (stratified).
- Resample or use SMOTE **only on the training fold**, inside a `Pipeline`, so cross-validation stays honest.
- Report **recall, PR-AUC and F1 on the stroke class** instead of accuracy, since missing a stroke is the costly error.

## Run it
```bash
git clone https://github.com/Ebraheem-zydan/Stroke-Prediction.git
cd Stroke-Prediction
pip install pandas numpy scikit-learn xgboost seaborn plotly missingno matplotlib
jupyter notebook NoteBook.ipynb
```

## Team
| Member | Focus |
|---|---|
| **Ibrahim Ragab** | Modeling: preprocessing pipeline, model selection, tuning, evaluation |
| Toka | Data analysis |
| Mariam Naeem | Data preprocessing |

Slides: [`Presentation.pptx`](Presentation.pptx)
