# Lesson 1: The ML Workflow

## Learning Goals

- `train_test_split`.
- `.fit()`, `.predict()`.

## 1. Data Splitting

Never test on the data you trained on.

- **Train Set (80%)**: To learn the parameters.
- **Test Set (20%)**: To evaluate performance on unseen data.

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

## 2. The Scikit-Learn API

Consistent API across all algorithms.

1.  **Instantiate**: `model = LinearRegression()`
2.  **Fit**: `model.fit(X_train, y_train)`
3.  **Predict**: `predictions = model.predict(X_test)`

## Key Takeaways

- The API is King. Scikit-learn's consistency is why it's the industry standard.
- Data Leakage often happens during splitting (e.g., time-series).
