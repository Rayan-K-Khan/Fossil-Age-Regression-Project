# 🦴 Fossil Age Prediction

A supervised machine learning project that predicts the **geological age of fossils** (in millions of years) using paleontological and geospatial features. Four regression models were benchmarked and optimized, with a tuned XGBoost achieving **R² = 0.990** and **RMSE = 9.55 Myr**.

---

## 📌 Project Overview

| | |
|---|---|
| **Goal** | Predict fossil age (Myr) from taxonomic and environmental features |
| **Type** | Supervised Regression |
| **Best Model** | Tuned XGBoost (R² = 0.990, RMSE = 9.55) |
| **Tools** | Python, scikit-learn, XGBoost, TensorFlow/Keras, pandas, seaborn |

---

## 📂 Dataset

The dataset contains paleontological records with the following features:

| Feature | Description |
|---|---|
| `taxon_name` | Species/genus name of the fossil |
| `class` | Biological class (e.g., Trilobita, Lingulata) |
| `phylum` | Biological phylum (e.g., Arthropoda, Brachiopoda) |
| `lithology` | Rock type the fossil was found in (e.g., shale, limestone) |
| `environment` | Depositional environment (e.g., marine indet.) |
| `latitude` | Geographic latitude of fossil discovery site |
| `longitude` | Geographic longitude of fossil discovery site |
| `age` *(target)* | Geological age in millions of years (Myr) |

---

## 🔧 Methodology

### 1. Exploratory Data Analysis (EDA)
- Distribution analysis via KDE plots and pairplot matrix
- Outlier detection on numerical features
- Correlation inspection between geospatial features and target age

### 2. Preprocessing
- **Categorical encoding**: Label encoding + one-hot encoding for `taxon_name`, `class`, `phylum`, `lithology`, `environment`
- **Feature scaling**: `StandardScaler` applied to all features
- **Train/test split**: Standard 80/20 stratified split

### 3. Models Trained

| Model | RMSE | R² |
|---|---|---|
| Lasso Regression | — | — |
| XGBoost (baseline) | ~20 | ~0.96 |
| Simple Neural Network | 110.42 | — |
| Complex Neural Network | 44.50 | — |
| **Tuned XGBoost** ✅ | **9.55** | **0.990** |

### 4. Hyperparameter Tuning
`RandomizedSearchCV` with 5-fold cross-validation was used to tune XGBoost over:

```python
{
    'n_estimators': randint(100, 400),
    'max_depth': randint(3, 10),
    'learning_rate': uniform(0.05, 0.3),
    'subsample': uniform(0.5, 0.5),
    'colsample_bytree': uniform(0.5, 0.5)
}
```

**Best parameters found:**
```
n_estimators    : 271
max_depth       : 6
learning_rate   : 0.1733
subsample       : 0.5697
colsample_bytree: 0.7377
```

### 5. Evaluation
- Metrics: MSE, RMSE, R²
- Residual scatter plots generated for all 4 models to assess bias and variance across the age range

---

## 📊 Results

The **tuned XGBoost regressor** was selected as the final model:

```
Tuned XGBoost Test MSE  : 91.16
Tuned XGBoost Test RMSE : 9.55
Tuned XGBoost Test R²   : 0.9900
```

Neural networks underperformed relative to XGBoost on this structured tabular dataset, consistent with the known strength of gradient boosting methods on feature-rich tabular data.

---

## 🛠️ Installation & Usage

```bash
# Clone the repo
git clone https://github.com/your-username/fossil-age-prediction.git
cd fossil-age-prediction

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook Fossil_Age_Project.ipynb
```

### Requirements
```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
tensorflow
statsmodels
```

---

## 📁 Repository Structure

```
fossil-age-prediction/
│
├── Fossil_Age_Project.ipynb   # Main analysis notebook
├── README.md                  # Project documentation
└── requirements.txt           # Python dependencies
```

---

## 🔍 Key Takeaways

- XGBoost significantly outperforms neural networks on this structured paleontological dataset
- Taxonomic features (`class`, `phylum`, `taxon_name`) are strong predictors of geological age
- Hyperparameter tuning via `RandomizedSearchCV` reduced RMSE by ~50% over the baseline XGBoost model
- Geospatial coordinates (`latitude`, `longitude`) provide additional signal beyond taxonomy alone
