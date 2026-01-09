# Lesson 4: Clustering

## Learning Goals

- K-Means.
- Centroids.

## 1. K-Means Algorithm

1.  Choose K random points as "Centroids".
2.  Assign every data point to the closest centroid.
3.  Move the centroid to the average center of its points.
4.  Repeat until convergence.

## 2. Selecting K (Elbow Method)

Plot "Inertia" (Sum of squared distances) vs K.
Look for the "Elbow" where adding more clusters gives diminishing returns.

## Key Takeaways

- Unsupervised learning has no labels. You don't know the "Right Answer".
- K-Means assumes spherical clusters.
