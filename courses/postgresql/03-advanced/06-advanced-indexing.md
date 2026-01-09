# Lesson 6: Advanced Indexing

## Learning Goals

- Go beyond B-Trees.
- Understand GIN, GiST, and BRIN indexes.
- Use Multiple-Column and Partial Indexes.

## 1. B-Tree (The Default)

- **Type**: Balanced Tree.
- **Use Case**: Equality (`=`) and Range (`<`, `>`).
- **Coverage**: 95% of your use cases.

## 2. GIN (Generalized Inverted Index)

- **Structure**: Maps a value to multiple rows (Inverted Index).
- **Use Case**: **JSONB** (Does this JSON doc contain key "x"?), **Arrays**, and **Full-Text Search**.
- **Perf**: Slower to update than B-Tree, but lightning fast for searching inside complex documents.

```sql
CREATE INDEX idx_data_gin ON documents USING GIN (data_jsonb);
```

## 3. GiST (Generalized Search Tree)

- **Use Case**: Geometric data (**PostGIS**), Nearest Neighbor search.
- Example: "Find all coffee shops within 5km of this point."

## 4. BRIN (Block Range Index)

- **Structure**: Only stores summary info (Min/Max values) for a range of physical pages. Extremely tiny index.
- **Use Case**: Very large tables (Terabytes) where data is physically ordered (e.g., Time Series logs, Sensor data).
- **Tradeoff**: Slower than B-Tree, but uses 1000x less RAM.

## 5. Advanced Techniques

**Multi-Column (Composite) Index**:
`CREATE INDEX idx_name ON users (last_name, first_name);`

- Good for `WHERE last_name = 'Smith' AND first_name = 'John'`.
- Good for `WHERE last_name = 'Smith'`.
- **Useless** for `WHERE first_name = 'John'` (Order matters! Left-to-Right rule).

**Partial Index**:
Only index a subset of rows to save space.

```sql
CREATE INDEX idx_todo_pending ON todos (created_at) WHERE status = 'PENDING';
```

If you have 1M completed tasks and 100 pending ones, this index is tiny and instant.

## Key Takeaways

- Use **GIN** for JSONB/Arrays.
- Use **GiST** for Geo/Spatial.
- Use **BRIN** for massive Time-Series logs.
- Use **Partial Indexes** to optimize work queues (status = 'active').
