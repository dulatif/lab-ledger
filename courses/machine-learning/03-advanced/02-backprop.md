# Lesson 2: Backpropagation

## Learning Goals

- The Chain Rule.
- Optimizers.

## 1. The Algorithm

1.  **Forward Pass**: Compute prediction.
2.  **Calculate Loss**: Difference between Prediction and Truth.
3.  **Backward Pass**: Calculate gradients (Who is to blame for the error?).
4.  **Update Weights**: Nudge weights to reduce error.

## 2. Optimizers

- **SGD (Stochastic Gradient Descent)**: Updates weights after every sample (noisy but fast).
- **Adam**: Adaptive Moment Estimation. The gold standard. It adapts the learning rate for each parameter.

## Key Takeaways

- You don't need to implement Backprop manually (Autograd handles it).
- Use `Adam` with default settings (lr=3e-4) as a starting point.
