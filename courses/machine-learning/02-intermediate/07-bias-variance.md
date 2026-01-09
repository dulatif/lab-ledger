# Lesson 7: Overfitting vs Underfitting

## Learning Goals

- Bias-Variance Tradeoff.
- Regularization.

## 1. Underfitting (High Bias)

Model is too simple to capture the pattern.
Performance is poor on both Training and Test data.

## 2. Overfitting (High Variance)

Model memorizes the Training data (including noise).
Performance is perfect on Training, terrible on Test.

## 3. The Sweet Spot

We want Generalization.

## 4. Regularization

Penalize complex models.

- **L1 (Lasso)**: Adds absolute value of weights to Loss. Can drive weights to 0 (Feature Selection).
- **L2 (Ridge)**: Adds squared value of weights to Loss. Keeps weights small.

## Key Takeaways

- If Train > Test, you are Overfitting -> Add Regularization, More Data, or Simplify Model.
- If Train and Test are both low, you are Underfitting -> Complexify Model.
