# Project Plan: Filling Missing Dairy Milk Production Data

**Course:** ANSC 4040 — Mini Project  
**Project Timeline:** September 15 – October 6, 2026  
**Development Environment:** Local computer using Visual Studio Code (VS Code) and Python

### Project objective

This part of the project predicts an animal identifier for each milking record in a dataset. The model uses milk-production and milking-session measurements as input features and predicts the animal number associated with the record.

The model predicts **animal identity**; it does not predict milk yield. The training data calls the target `AnimalNumber`. In the prediction output, that predicted value is labeled `AnimalId`.

### Data sources and preparation

The workflow uses two datasets:

- **Labeled training data:** `project_1_ANSC_4040_dataset.csv`, which contains `AnimalNumber` labels.
- **Updated records to predict:** `PracticeTest.csv`, which contains `MilkingId` and session measurements but no animal identifier.

The updated data is cleaned in `data_inspection.ipynb` by:

1. Removing exact duplicate rows.
2. Converting the five milk-production/session features to numeric values, coercing invalid values to missing:
   - `Avgmilkflow`
   - `Flow30_60Session`
   - `YieldFirst2Min_Session`
   - `YieldSession`
   - `DurationSession_sec`
3. Removing rows missing `Avgmilkflow` or any of the five model features.
4. For each of those five features, removes rows more than two standard deviations from that feature's mean. The filters are applied sequentially.
5. Saves the cleaned records as `PracticeTest_cleaned.csv`.

For the current replacement file, inspection started with **1,701,003 rows**. It removed **29 rows** with missing `Avgmilkflow`; it found no exact duplicate rows and no further rows missing model features. The sequential outlier filters removed additional rows, leaving **1,404,392 cleaned records**. The original `PracticeTest.csv` is left unchanged.

The labeled training data is loaded separately by the KNN notebook. Rows without an `AnimalNumber` label are excluded. The model uses a random sample of up to **500,000 labeled rows** for training and evaluation.

### Features and target

The target is `AnimalNumber` from the labeled training data. The predicted output column is named `PredictedAnimalId`, and is renamed to `AnimalId` in the two-column mapping file.

The notebook selects features that are both present and usable in the training and prediction data. For this run, it used:

- `Avgmilkflow`
- `Flow30_60Session`
- `YieldFirst2Min_Session`
- `YieldSession`
- `DurationSession_sec`
- `milking`

Although `EventDate` is present in the replacement records, it is not used because it is not available in the selected training dataset. Other metadata fields are not used when they have no usable values in the replacement data.

### Model choice and preprocessing

The model is a **K-Nearest Neighbors (KNN) classifier** implemented with scikit-learn. KNN assigns a class based on nearby training examples in feature space. It is used here to select an animal identifier from the animal numbers present in the training data.

The notebook processes numeric features with a pipeline that:

1. Imputes missing values using the training-set median and adds missingness indicators.
2. Standardizes feature scales with `StandardScaler`, since KNN uses distances between feature values.
3. Applies `KNeighborsClassifier` to assign an animal-number class.

The notebook also contains categorical-feature preprocessing for columns that require it; no categorical features were used in this run.

### Training, evaluation, and hyperparameters

The labeled data is split into training and test subsets using an **80/20 random split** with `random_state=42`. The classifier is fitted on the training portion and evaluated on the held-out portion.

The configured KNN hyperparameter is **`n_neighbors=20`**. The notebook uses up to **500,000 training rows** and predicts in batches of **25,000 records**.

**Hyperparameter tuning was not performed.** The value of `n_neighbors` was fixed at 20; the notebook does not compare multiple values or select parameters through cross-validation. The reported metric is accuracy on the random holdout split.

For the current run:

- Training sample: **500,000 rows**
- Animal-number classes represented in the training sample: **8,930**
- Holdout accuracy: **0.0037 (0.37%)**

### Outputs

The notebook produces:

- `PracticeTest_cleaned.csv` — cleaned updated records used for prediction.
- `PracticeTest_knn_predictions.csv` — the cleaned records with a `PredictedAnimalId` column added.
- `MilkingId_AnimalId_predictions.csv` — the mapping with exactly two columns: `MilkingId` and `AnimalId`.

The current mapping contains **1,404,392 rows**, one for each cleaned record. The `AnimalId` in this mapping is the model's prediction, not a ground-truth label from the updated dataset.

### Limitations and interpretation

The holdout accuracy of **0.37% is low**. This means the current model has not demonstrated reliable animal identification. The mapping should be treated as experimental output, not as verified animal-to-milking assignments.

The target has thousands of possible animal-number classes, while the model uses only the listed session measurements. More evaluation and model development are needed before relying on its predictions. In particular, future work should compare KNN settings and alternative methods, establish suitable baselines, and evaluate generalization using a split that avoids overly optimistic results from related records appearing in both training and test sets.
