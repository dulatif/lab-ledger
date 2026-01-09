# Lesson 5: Dimensionality Reduction

## Learning Goals

- PCA.
- Curse of Dimensionality.

## 1. The Curse of Dimensionality

As you add more features (columns), the data becomes sparse. Euclidean distance loses meaning. You need exponentially more data to fill the space.

## 2. PCA (Principal Component Analysis)

Projects data onto a lower-dimensional space while preserving the maximum **Variance**.
Compresses 100 columns into 10 "Principal Components" that capture 95% of the information.

## Key Takeaways

- Use PCA to visualize high-dimensional data (reduce to 2D/3D).
- It speeds up training by removing noise and redundant features.
