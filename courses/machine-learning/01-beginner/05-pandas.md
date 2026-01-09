# Lesson 5: Pandas

## Learning Goals

- `DataFrame`.
- Filtering & Grouping.

## 1. The DataFrame

A tabular data structure (Excel in Python).
Columns have names, Rows have Indices.

```python
import pandas as pd
df = pd.read_csv("data.csv")
```

## 2. Inspection

- `df.head()`: View first 5 rows.
- `df.info()`: Check types and missing values.
- `df.describe()`: Statistical summary.

## 3. Manipulation

- `df['col']`: Select column.
- `df[df['age'] > 20]`: Filter rows.
- `df.groupby('category').mean()`: Aggregate.

## Key Takeaways

- Pandas is the Swiss Army Knife of Data Science.
- Master `loc` and `iloc` for indexing.
