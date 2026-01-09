# Lesson 2: Regression

## Learning Goals

- Linear Regression.
- Gradient Descent.

## 1. The Model

Predicting a continuous number (Price, Temperature).
$y = mx + b$ (or $y = w \cdot x + b$)

- $w$: Weights (Slopes).
- $b$: Bias (Intercept).

## 2. Gradient Descent

How do we find the best $w$ and $b$?

1.  Start with random weights.
2.  Calculate Error (MSE).
3.  Calculate Gradient (Direction of steepest error).
4.  Update weights: $w_{new} = w_{old} - learning\_rate * gradient$.
5.  Repeat.

## Key Takeaways

- Regression is the "Hello World" of ML.
- Assumes a linear relationship (can be fixed with Polynomial features).
