# Lesson 6: Visualization

## Learning Goals

- Matplotlib.
- Seaborn.

## 1. Matplotlib

The low-level plotting library. Powerful but verbose.
`plt.plot(x, y)`

## 2. Seaborn

High-level wrapper around Matplotlib. Beautiful defaults.

```python
import seaborn as sns
sns.scatterplot(data=df, x="age", y="salary", hue="gender")
```

## 3. Types of Plots

- **Histogram**: Distribution of a single variable.
- **Scatter Plot**: Relationship between two variables.
- **Heatmap**: Correlation matrix.
- **Box Plot**: Outliers and quartiles.

## Key Takeaways

- Always visualize your data _before_ modeling (Exploratory Data Analysis).
- Anscombe's Quartet proves that stats alone can be misleading.
