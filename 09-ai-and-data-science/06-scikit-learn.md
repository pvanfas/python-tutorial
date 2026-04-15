# Chapter 54: scikit-learn — Your ML Toolkit

> scikit-learn gives you every major ML algorithm in 3 lines of code. The hard math is already done.

---

## 🎯 What You'll Learn

- The scikit-learn API (it's beautifully consistent)
- Training a classification model
- Training a regression model
- Evaluating model performance
- Feature scaling and preprocessing

---

## 📦 Installation

```bash
pip install scikit-learn
```

```python
# Common imports
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report, mean_squared_error
```

---

## 🎨 The scikit-learn API Pattern

Every algorithm in scikit-learn follows the same 3-step pattern:

```python
# Step 1: Create the model
model = SomeAlgorithm()

# Step 2: Train (fit) the model on training data
model.fit(X_train, y_train)

# Step 3: Make predictions
predictions = model.predict(X_test)
```

This consistency is one of scikit-learn's greatest strengths. Learn it once, apply it everywhere.

---

## 🏠 Example 1: Regression — Predicting House Prices

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# Simulated housing data
np.random.seed(42)
n = 200

data = pd.DataFrame({
    "size_sqft":  np.random.randint(500, 3000, n),
    "bedrooms":   np.random.randint(1, 6, n),
    "age_years":  np.random.randint(1, 40, n),
    "distance_km": np.random.randint(1, 30, n),
})

# Create house price with some logic + noise
data["price_lakhs"] = (
    data["size_sqft"] * 0.04 +
    data["bedrooms"] * 2 +
    -data["age_years"] * 0.5 +
    -data["distance_km"] * 0.8 +
    np.random.normal(0, 5, n)   # Random noise
)

# Prepare features (X) and target (y)
X = data[["size_sqft", "bedrooms", "age_years", "distance_km"]]
y = data["price_lakhs"]

# Split: 80% training, 20% testing
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print(f"Training samples: {len(X_train)}")
print(f"Test samples:     {len(X_test)}")

# Train the model
model = LinearRegression()
model.fit(X_train, y_train)

# Make predictions on test set
y_pred = model.predict(X_test)

# Evaluate
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_test, y_pred)

print(f"\n📊 Model Performance:")
print(f"  RMSE:  {rmse:.2f} lakhs  (avg prediction error)")
print(f"  R²:    {r2:.4f}  (1.0 = perfect, 0 = random)")

# Show feature importance
print(f"\n📐 Feature Coefficients:")
for feature, coef in zip(X.columns, model.coef_):
    print(f"  {feature}: {coef:.4f}")

# Predict for a new house
new_house = [[1200, 3, 10, 5]]  # 1200 sqft, 3 beds, 10 years old, 5 km away
predicted = model.predict(new_house)[0]
print(f"\n🏠 New house prediction: ₹{predicted:.2f} lakhs")
```

---

## 🌸 Example 2: Classification — Iris Flower Dataset

The Iris dataset is the "Hello World" of machine learning — classifying flowers by petal measurements.

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

# Load the famous Iris dataset (built into scikit-learn)
iris = load_iris()
X = iris.data       # 4 features: sepal length, sepal width, petal length, petal width
y = iris.target     # 3 classes: 0=Setosa, 1=Versicolor, 2=Virginica

print(f"Dataset shape: {X.shape}")   # → (150, 4)
print(f"Classes: {iris.target_names}")

# Split the data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Train a Random Forest
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Predict
y_pred = model.predict(X_test)

# Evaluate
accuracy = accuracy_score(y_test, y_pred)
print(f"\n✅ Accuracy: {accuracy * 100:.2f}%")

print("\n📊 Detailed Report:")
print(classification_report(y_test, y_pred, target_names=iris.target_names))

# Feature importance
print("📐 Feature Importance:")
for feature, importance in zip(iris.feature_names, model.feature_importances_):
    print(f"  {feature}: {importance:.4f}")

# Predict a new flower
new_flower = [[5.1, 3.5, 1.4, 0.2]]   # Measurements
prediction = model.predict(new_flower)[0]
print(f"\n🌸 Predicted flower: {iris.target_names[prediction]}")
```

---

## 🔄 Feature Scaling — Why It Matters

Some algorithms (like Linear Regression, SVM, KNN) are sensitive to feature scale. If one feature ranges from 0–1 and another from 0–1,000,000, the large-scale feature dominates.

**StandardScaler** transforms features to have mean=0 and std=1:

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

# Fit scaler on TRAINING data only (not test data — that would be cheating!)
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)   # Use same scaler, just transform

# Now train your model on scaled data
model.fit(X_train_scaled, y_train)
predictions = model.predict(X_test_scaled)
```

> ⚠️ **Common mistake:** Never fit the scaler on the test data. The test data should be treated as "unseen" — you only `transform()` it, never `fit_transform()`.

---

## 📊 Understanding Evaluation Metrics

### For Classification:
```
              Predicted: Cat  Predicted: Dog
Actual: Cat       45 (TP)         5 (FN)
Actual: Dog        3 (FP)        47 (TN)

Accuracy  = (TP + TN) / Total  = 92/100 = 92%
Precision = TP / (TP + FP)     = 45/48 = 93.75%
Recall    = TP / (TP + FN)     = 45/50 = 90%
F1-Score  = 2 × (P × R)/(P+R) = 91.8%
```

### For Regression:
- **MSE** (Mean Squared Error): Average of squared errors. Lower = better.
- **RMSE** (Root MSE): Same units as your target. More interpretable.
- **R² Score**: 1.0 = perfect, 0 = no better than guessing the mean.

---

## 🤖 Comparing Multiple Models

```python
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.svm import SVC
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(
    iris.data, iris.target, test_size=0.2, random_state=42
)

models = {
    "Logistic Regression":  LogisticRegression(max_iter=200),
    "Decision Tree":        DecisionTreeClassifier(),
    "Random Forest":        RandomForestClassifier(n_estimators=100),
    "K-Nearest Neighbors":  KNeighborsClassifier(),
    "Support Vector Machine": SVC(),
}

print(f"{'Model':<30} {'Accuracy':>10}")
print("-" * 42)

for name, model in models.items():
    model.fit(X_train, y_train)
    acc = accuracy_score(y_test, model.predict(X_test))
    print(f"{name:<30} {acc * 100:>9.2f}%")
```

---

## 🏋️ Activity: Predict Student Performance

Build a full ML pipeline:

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import accuracy_score, classification_report

# Generate student dataset
np.random.seed(42)
n = 500

df = pd.DataFrame({
    "study_hours":   np.random.uniform(0, 10, n),
    "attendance":    np.random.uniform(50, 100, n),
    "sleep_hours":   np.random.uniform(4, 10, n),
    "prev_score":    np.random.uniform(30, 100, n),
    "extra_classes": np.random.choice([0, 1], n),
})

# Target: Will the student pass? (score > 50)
df["pass"] = (
    (df["study_hours"] * 5 +
     df["attendance"] * 0.3 +
     df["prev_score"] * 0.5 +
     df["extra_classes"] * 10 +
     np.random.normal(0, 10, n)) > 80
).astype(int)

print(f"Pass rate: {df['pass'].mean() * 100:.1f}%")

X = df.drop("pass", axis=1)
y = df["pass"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(f"\n✅ Accuracy: {accuracy_score(y_test, y_pred) * 100:.2f}%")

# Feature importance — which factors matter most?
print("\n📊 Most Important Factors for Passing:")
for feat, imp in sorted(zip(X.columns, model.feature_importances_),
                         key=lambda x: x[1], reverse=True):
    bar = "█" * int(imp * 40)
    print(f"  {feat:<20} {bar} {imp:.3f}")

# Predict for a specific student
student = pd.DataFrame([[8, 90, 7, 75, 1]],
                        columns=X.columns)
prediction = model.predict(student)[0]
probability = model.predict_proba(student)[0]
print(f"\n👤 Student prediction: {'PASS ✅' if prediction == 1 else 'FAIL ❌'}")
print(f"   Confidence: {max(probability) * 100:.1f}%")
```

---

## 🧠 Check Your Understanding

1. What are the three steps of the scikit-learn API?
2. Why do we split data into training and test sets?
3. What is feature scaling and when is it needed?
4. What is the difference between accuracy and F1-score?
5. What does `model.predict_proba()` return?

---

## 📌 Key Takeaways

- scikit-learn's consistent API: **create → fit → predict**
- Always split data: `train_test_split(X, y, test_size=0.2)`
- Scale features for distance-based algorithms
- Compare multiple algorithms to find the best one
- Evaluate with appropriate metrics for your problem type

---

*Next: [Neural Networks — The Big Picture →](07-neural-networks.md)*
