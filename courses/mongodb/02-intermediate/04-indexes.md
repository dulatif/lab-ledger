# Lesson 4: Single & Compound Indexes

## Learning Goals

- B-Trees.
- ESR Rule.

## 1. Indexes Explained

Without an index, MongoDB performs a **Collection Scan** (reads every document).
With an index, it traverses a B-Tree (Logarithmic time).

## 2. Single Field Index

`db.users.createIndex({ name: 1 })`.
Speeds up queries on `name` and sorts on `name`.

## 3. Compound Index

`db.users.createIndex({ age: 1, name: 1 })`.
Order matters!

- Can support query on `age`.
- Can support query on `age` AND `name`.
- **Cannot** support query on just `name` (efficiently).

## 4. The ESR Rule

For optimal Compound Indexes, organize fields in this order:

1.  **Equality**: `status = "Active"`
2.  **Sort**: `sort({ date: -1 })`
3.  **Range**: `age > 21`

## Key Takeaways

- Indexes come at a cost (Write performance degradation).
- Always ensure your "Operational Queries" are indexed.
