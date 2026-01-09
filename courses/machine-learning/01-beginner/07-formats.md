# Lesson 7: Data Collection & Formats

## Learning Goals

- CSV/JSON.
- Databases.

## 1. File Formats

- **CSV**: Comma Separated. Simple, human readable. No types.
- **JSON**: Nested structures. Web API standard.
- **Parquet**: Columnar storage. Binary. Compressed. Good for Big Data.

## 2. SQL

Using `pd.read_sql(query, connection)` to pull directly from databases.

## 3. APIs

Using `requests` to fetch JSON data from REST APIs.

## Key Takeaways

- Prefer Parquet for large local datasets (faster reads, smaller size).
- Always inspect CSV encoding (UTF-8 vs Latin-1).
