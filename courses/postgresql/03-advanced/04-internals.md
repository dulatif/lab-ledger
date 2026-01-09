# Lesson 4: Internals Deep Dive

## Learning Goals

- Understand Pages, Blocks, and Heap Tables.
- Understand TOAST (The Oversized-Attribute Storage Technique).
- Explain the lifecycle of a query.

## 1. Storage Hierarchy

How does Postgres store data on the NVMe drive?

1.  **Files**: Each table is a file (or multiple 1GB segment files) on disk.
2.  **Pages (Blocks)**: The file is divided into fixed-size **8KB Pages**.
3.  **Tuples (Rows)**: Rows are stored inside these pages.

**Reading Data**:
Postgres cannot read "half a row". It reads the **Entire 8KB Page** from disk into the **Shared Buffers** (RAM).

- If you select 1 byte from a 1TB table, and it's not in RAM, Postgres must fetch at least one 8KB block.

## 2. TOAST (Handling Large Data)

What happens if you try to store a 5MB JSON document in a row? A page is only 8KB!

**TOAST (The Oversized-Attribute Storage Technique)**

- Postgres automatically detects large fields.
- It compresses them.
- If still too big, it slices them into chunks and stores them in a separate "TOAST Table".
- The main table row just stores a pointer to the TOAST table.
- **Impact**: `SELECT *` on large columns forces Postgres to fetch from the TOAST table (extra I/O).

## 3. Query Lifecycle

1.  **Parser**: Checks SQL syntax.
2.  **Rewriter**: Applies Views and Rules.
3.  **Planner (Optimizer)**: The Brain. It looks at statistics (row counts) and decides:
    - "Should I use an Index or Scan the whole table?"
    - "Should I loop join or hash join?"
    - It generates a "Query Plan".
4.  **Executor**: Executes the plan and returns rows.

## Key Takeaways

- **8KB Pages**: The fundamental unit of I/O.
- **Shared Buffers**: The RAM cache where pages live.
- **TOAST**: Keeps the main table small by offloading large text/binary data.
- **The Planner**: The most important component for performance; it relies on accurate statistics (ANALYZE).
