# Lesson 8: Preprocessing & Cleaning

## Learning Goals

- Missing Values.
- Outliers.

## 1. Missing Values (NaN)

- **Drop**: `df.dropna()`. Good if you have lots of data.
- **Impute**: `df.fillna(df.mean())`. Fill with average or median.

## 2. Duplicates

`df.drop_duplicates()`.

## 3. Outliers

Data points that are far from the rest.
Can be errors or anomalies (fraud).
Detect using Z-Score or IQR (Interquartile Range).

## Key Takeaways

- Cleaning takes 80% of the time. Modeling takes 20%.
- Never impute based on Test Data stats (Data Leakage).
