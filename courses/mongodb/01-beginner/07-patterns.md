# Lesson 7: Schema Design Patterns

## Learning Goals

- Attribute Pattern.
- Bucket Pattern.
- Polymorphic Pattern.

## 1. The Attribute Pattern

Problem: Many similar fields (`price_usd`, `price_eur`, `price_gbp`) require many indexes.
Solution: Array of Key-Value pairs.
`attributes: [ { k: "currency", v: "USD" }, { k: "price", v: 100 } ]`.

## 2. The Bucket Pattern

Problem: Time series data generates too many small documents.
Solution: Group data into "Buckets" (e.g., 1 document per hour).
`{ sensorId: 1, date: "2024-01-01", readings: [23, 24, 21, ...] }`.

## 3. The Polymorphic Pattern

Problem: Products have different fields (Book has ISBN, Phone has Screen Size).
Solution: Store them in the same collection. Identify type with a `type` field.
Application logic handles the differences.

## Key Takeaways

- Patterns solve specific scale/performance problems.
- Don't optimize prematurely.
