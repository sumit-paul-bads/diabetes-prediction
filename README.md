# 🩺 Diabetes Prediction using Machine Learning

## 📌 Overview
This project builds a machine learning pipeline to predict whether a patient is likely to have diabetes based on clinical features.  
The focus is not just model accuracy, but **making decisions aligned with healthcare priorities**, where **minimizing false negatives (high recall)** is critical.

---

## 📊 Dataset
- PIMA Indians Diabetes Dataset
- ~700+ records
- Features include:
  - Pregnancies
  - Glucose
  - Blood Pressure
  - Skin Thickness
  - Insulin
  - BMI
  - Diabetes Pedigree Function
  - Age

---

## ⚙️ Data Preprocessing

### 🔹 Handling Missing Values
Certain features contained invalid zero values (which are not medically possible).  
These were treated as missing and imputed:

- Columns handled:
  - Glucose
  - BloodPressure
  - SkinThickness
  - Insulin
  - BMI

- Strategy:
  - Replace `0 → NaN`
  - Fill with **median values**

---

## 🧠 Feature Engineering (Domain-Based)

Instead of relying only on raw features, domain-driven features were created:

- **BMI_Age** → captures combined risk of obesity and age  
- **Glucose_BMI** → interaction between glucose level and body mass  
- **Preg_Age** → pregnancies adjusted with age  
- **High_Glucose** → binary flag for high glucose levels  
- **Is_Obese** → binary obesity indicator based on BMI  

👉 These features help capture **non-linear and interaction effects**.

---

## 🔍 Feature Selection

Correlation analysis was used to remove redundant features and avoid multicollinearity:

### ❌ Dropped Features:
- **Glucose**
  - Highly correlated with `Glucose_BMI`
  - Lower predictive power compared to engineered feature

- **Preg_Age**
  - Very high correlation (~0.95) with Pregnancies → duplicate signal

- **Is_Obese**
  - Derived from BMI and highly correlated (~0.76)
  - Less informative than continuous BMI

👉 Goal: **reduce redundancy while keeping strongest signals**

---

## 📏 Feature Scaling

- Applied **StandardScaler**
- Important for:
  - Logistic Regression
  - SVM
- Not required but harmless for:
  - Tree-based models (Random Forest, XGBoost)

---

## 🤖 Model Training

Multiple models were trained and compared:

- Logistic Regression
- Support Vector Machine (SVM)
- Random Forest
- Decision Tree
- XGBoost

### 🔧 Techniques Used:
- Train-test split
- 5-fold Cross Validation
- RandomizedSearchCV for hyperparameter tuning
- Evaluation on unseen test data

---

## 📈 Evaluation Metrics

Models were evaluated using:

- ROC-AUC
- Precision
- Recall ⭐ (most important)
- F1-score
- Confusion Matrix

---

## 📊 Results Summary

| Model | Test AUC | Precision | Recall | F1 Score |
|------|--------|----------|--------|----------|
| Logistic Regression | ~0.81 | ~0.59 | 0.50 | 0.54 |
| SVM | ~0.80 | ~0.65 | ~0.48 | ~0.55 |
| Random Forest | ~0.80 | ~0.62 | **~0.52 (Highest)** | ~0.56 |
| XGBoost | ~0.80 | ~0.57 | ~0.48 | ~0.52 |
| Decision Tree | ~0.76 | ~0.64 | ~0.44 | ~0.52 |

---

## 🏆 Model Selection (Key Insight)

Although **Logistic Regression had the highest AUC**, it produced more **false negatives**, which is risky in healthcare.

👉 In medical diagnosis:
> Missing a positive case (false negative) is worse than a false alarm.

### ✅ Final Choice: **Random Forest**
- Highest recall (least false negatives)
- Better balance between sensitivity and performance
- Robust to noise and feature interactions

---

## 📉 Confusion Matrix Insight (Random Forest)

- Lower false negatives compared to other models
- Better identification of diabetic patients

---

## 📦 Output

Predictions were exported to CSV containing:

- Input features
- Actual outcome
- Predicted outcome
- Prediction probability
- Correct/Incorrect flag

---

## 📁 Project Structure

diabetes-prediction/ │ ├── notebooks/ │   └── diabetes_prediction_pipeline.ipynb │ ├── models/ │   └── random_forest.pkl │ ├── outputs/ │   └── diabetes_predictions.csv │ └── README.md

---

## 🛠️ Tech Stack

- Python
- Pandas, NumPy
- Scikit-learn
- XGBoost
- Matplotlib, Seaborn

---

## 🚀 Key Takeaways

- Domain-driven feature engineering improves model performance
- Feature selection reduces redundancy and improves generalization
- Model selection should consider **business context**, not just metrics
- In healthcare:
  - **Recall > Accuracy**

---

## 🔮 Future Improvements

- Handle class imbalance (SMOTE / class weights)
- Perform deeper hyperparameter tuning
- Try ensemble stacking
- Deploy model using Streamlit / FastAPI
- Add explainability (SHAP / LIME)

---

## 👨‍💻 Author
**Sumit Paul**

---

## ⭐ If you found this useful, consider starring the repo!
