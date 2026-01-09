# Lesson 2: Core Concepts

## Learning Goals

- Define the hierarchy of PostgreSQL objects (Cluster -> Database -> Schema -> Table).
- Understand Tables, Rows, and Columns.
- Explore essential Data Types.

## 1. The PostgreSQL Hierarchy

When you interact with PostgreSQL, you are interacting with a hierarchy of containers:

1.  **Cluster**: A collection of databases managed by a single PostgreSQL server instance. It allows access to data.
2.  **Database**: An isolated container within a cluster. Users connect to a specific database.
3.  **Schema**: A logical namespace within a database for grouping objects (tables, views, functions).
    - Think of it like folders in a file system.
    - The default schema is `public`.
4.  **Table**: The actual storage unit containing data.

**Analogy:**

- **Cluster** = An Office Building.
- **Database** = A Company occupying a floor.
- **Schema** = A Department (HR, Sales) within the company.
- **Table** = A Filing Cabinet in that department.

## 2. Tables, Rows, and Columns

- **Table (Relation)**: A collection of related data entries.
  - Example: `employees`
- **Column (Attribute)**: A specific piece of data stored for each record. It has a name and a data type.
  - Example: `first_name`, `salary`, `hire_date`
- **Row (Tuple/Record)**: A single set of data describing one entity.
  - Example: One specific employee's details.

## 3. Essential Data Types

PostgreSQL is famous for its rich set of data types. Choosing the right one is critical for performance and data integrity.

### Numeric Types

- `INTEGER`: Standard integer (4 bytes). range: -2 billion to +2 billion.
- `BIGINT`: Large integer (8 bytes). Use this for IDs or whenever `INTEGER` might overflow.
- `NUMERIC` / `DECIMAL`: Exact precision numbers. **Mandatory for money** to avoid floating-point math errors.
- `REAL` / `DOUBLE PRECISION`: Floating-point numbers. Fast, but less precise. Good for scientific data.

### Character Types

- `TEXT`: Variable length text. The most versatile and recommended string type in Postgres.
- `VARCHAR(n)`: Variable length with a limit. Only use if you _strictly_ need a length constraint.
- `CHAR(n)`: Fixed length. Padding is added if the string is short. Rarely used in modern schemas.

### Date/Time Types

- `TIMESTAMP`: Date and Time (e.g., `2023-10-27 14:30:00`).
- `TIMESTAMPTZ` (**Best Practice**): Timestamp _with time zone_. It converts input to UTC for storage and converts back to the client's time zone for display. Always prefer this over `TIMESTAMP`.
- `DATE`: Date only (`2023-10-27`).
- `INTERVAL`: A duration of time (`1 day`, `2 hours`).

### Boolean

- `BOOLEAN`: `TRUE`, `FALSE`, or `NULL`.

### Special Types

- `UUID`: Universally Unique Identifier. Great for distributed systems keys.
- `JSONB`: Binary JSON. Stores JSON data in a decomposed binary format that supports indexing.
- `ARRAY`: Postgres enables columns to store arrays of other types (e.g., `INTEGER[]`).

## Key Takeaways

- Data is organized: Instance -> Database -> Schema -> Table.
- The default schema is `public`, but using custom schemes (like `app`, `auth`) is good practice.
- **Always** use `TIMESTAMPTZ` for time.
- **Always** use `NUMERIC` for currency.
- Prefer `TEXT` over `VARCHAR` unless you have a specific business rule limiting length.
