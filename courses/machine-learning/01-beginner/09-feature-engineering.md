# Lesson 9: Feature Engineering

## Learning Goals

- Scaling.
- Encoding.

## 1. Scaling

Distance-based models (KNN, SVM) require features on the same scale.

- **MinMax**: 0 to 1.
- **Standardization (Z-Score)**: Mean 0, Std 1.

## 2. Encoding Categorical Data

Machines only understand numbers.

- **One-Hot Encoding**: `cat` -> `[1, 0]`, `dog` -> `[0, 1]`. Good for low cardinality.
- **Label Encoding**: `small` -> 0, `medium` -> 1, `large` -> 2. Good for ordinal data.

## Key Takeaways

- Feature Engineering is where you inject domain knowledge.
- Good features beat good algorithms.
