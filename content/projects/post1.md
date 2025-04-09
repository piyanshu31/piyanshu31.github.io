+++
title = "Temperature vs Power Analysis"
date = 2025-04-06
summary = "Analyzing power generation variation across temperature, humidity, and wick counts."
tags = ["Machine Learning", "Analysis"]
draft = false
link = "/projects/post1/"
hiddenFromHomePage= false
+++

## 📌 Summary

This project investigates how **temperature**, **humidity**, and **cotton wick count** affect **power output**. Rather than assuming a linear relationship, I explored **non-linear modeling** using **Polynomial Regression** and **LASSO (L1) regularization** to ensure interpretability and avoid overfitting.

---

## 🧪 Objective

- Predict power output using real-world experimental data
- Capture **non-linear** dependencies between features and target
- Evaluate the effectiveness of **regularized regression**

---

## 🧰 Tools & Libraries

- Python
- Pandas, NumPy
- Scikit-learn
- Matplotlib, Seaborn

---

## 📊 Dataset Overview

The dataset contains the following columns:

- `temperature` (in °C)
- `humidity` (%)
- `wick_count` (number of cotton wicks)
- `power_output` (watts)

Here’s a quick look at the data:

```python
import pandas as pd

df = pd.read_csv("power_dataset.csv")
print(df.head())
```
---

## 🔍 Feature Engineering & Modeling

### 1. Polynomial Transformation

To capture non-linearity:

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(df[['temperature', 'humidity', 'wick_count']])
```
---

## 📉 2. LASSO Regression

```python
from sklearn.linear_model import Lasso
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

X_train, X_test, y_train, y_test = train_test_split(
    X_poly, df['power_output'], test_size=0.2, random_state=42
)

lasso = Lasso(alpha=0.1)
lasso.fit(X_train, y_train)

y_pred = lasso.predict(X_test)
print("MSE:", mean_squared_error(y_test, y_pred))
```

---

## 📊 3. Visualization

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.regplot(x=y_test, y=y_pred, scatter_kws={"alpha": 0.6})
plt.xlabel("Actual Power Output")
plt.ylabel("Predicted Power Output")
plt.title("LASSO Regression Predictions")
plt.grid(True)
plt.show()
```

---

## 💡 Key Insights

- LASSO helped suppress insignificant polynomial terms.  
- Wick count had a **non-linear impact** on power — more isn’t always better.  
- Regularization was critical to prevent **overfitting** due to the expanded feature space.

---

## 🔗 References & Further Reading

- [Scikit-learn: Lasso Regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)  
- [Understanding Polynomial Features](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html)  
- [Bias-Variance Tradeoff](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)
