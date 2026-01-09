# Lesson 7: Basic SQL Operations

## Learning Goals

- Deepen understanding of creating tables and schemas.
- Differentiate between DDL (Definition) and DML (Manipulation).
- Execute standard CRUD operations.

## 1. DDL: Data Definition

DDL defines the _structure_ of your data.

**Creating a Schema**:
Good practice suggests not dumping everything in public.

```sql
CREATE SCHEMA app_data;
```

**Creating a Table**:

```sql
CREATE TABLE app_data.users (
    id SERIAL PRIMARY KEY,            -- Auto-incrementing Integer
    email TEXT UNIQUE NOT NULL,       -- Duplicate emails blocked
    full_name TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW() -- Auto-timestamp
);
```

**Modifying a Table**:

```sql
ALTER TABLE app_data.users ADD COLUMN is_active BOOLEAN DEFAULT TRUE;
```

## 2. DML: Data Manipulation

DML handles the _data_ inside the structures.

**INSERT (Create)**:

```sql
INSERT INTO app_data.users (email, full_name)
VALUES ('alice@example.com', 'Alice Wonderland');

-- Insert multiple
INSERT INTO app_data.users (email, full_name) VALUES
('bob@example.com', 'Bob Builder'),
('charlie@example.com', 'Charlie Chaplin');
```

**RETURNING Clause**:
Postgres allows you to get data back immediately after an Insert/Update/Delete. Helpful for getting generated IDs.

```sql
INSERT INTO app_data.users (email, full_name)
VALUES ('dave@example.com', 'Dave')
RETURNING id, created_at;
```

**SELECT (Read)**:

```sql
SELECT * FROM app_data.users WHERE is_active = TRUE;
```

**UPDATE (Update)**:
_Always_ use a WHERE clause!

```sql
UPDATE app_data.users
SET is_active = FALSE
WHERE email = 'bob@example.com';
```

**DELETE (Delete)**:

```sql
DELETE FROM app_data.users WHERE id = 1;
```

## 3. Best Practices

1.  **Lower Case**: Write table/column names in `snake_case`. Postgres forces quotes if you want "CaseSensitive" names, which is annoying.
    - Bad: `UserAccounts`, `FirstName`
    - Good: `user_accounts`, `first_name`
2.  **Explicit Columns**: Avoid `SELECT *` in production apps. List the columns you need.
3.  **Transactions**: If inserting into multiple tables (e.g., User and Profile), wrap them in `BEGIN...COMMIT` to ensure data integrity.

## Key Takeaways

- **DDL** creates objects (`CREATE`, `ALTER`, `DROP`).
- **DML** changes data (`INSERT`, `UPDATE`, `DELETE`).
- The `RETURNING` clause is a powerful Postgres feature to avoid a second round-trip query.
