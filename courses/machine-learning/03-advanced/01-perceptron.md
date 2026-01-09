# Lesson 1: The Perceptron

## Learning Goals

- Neurons.
- Activation Functions.

## 1. The Artificial Neuron

Modeled after the brain.
$Output = Activation(Sum(Input_i \cdot Weight_i) + Bias)$

## 2. Universal Approximation Theorem

A neural network with enough hidden units can approximate _any_ continuous function.
This is why they are so powerful.

## 3. Activation Functions

They introduce Non-Linearity. Without them, a Deep NN is just a single Linear Regression.

- **sigmoid**: 0 to 1. (Legacy, vanishes gradients).
- **ReLU (Rectified Linear Unit)**: `max(0, x)`. The standard today. Fast and avoids vanishing gradient.

## Key Takeaways

- Deep Learning is just "Stacking layers of perceptrons".
- Always use ReLU for hidden layers.
