# Lesson 2: Authorization & Roles

## Learning Goals

- Understand the difference between Users and Groups (they are both Roles).
- Manage privileges using `GRANT` and `REVOKE`.
- Use `DEFAULT PRIVILEGES` to automate permission handling.

## 1. Roles: Users vs Groups

In PostgreSQL, "User" and "Group" are the same thing: a **Role**.
The only difference is that a "User" usually has the `LOGIN` attribute.

```sql
-- Create a "Group" role (cannot login)
CREATE ROLE read_only_staff;

-- Create a "User" role (can login, inherits from group)
CREATE ROLE alice WITH LOGIN PASSWORD 'secret' IN GROUP read_only_staff;
```

## 2. Object Privileges (GRANT/REVOKE)

Privileges must be granted explicitly for each object type (Table, Schema, Sequence).

**Granting Usage**:
First, the user needs to attain the schema to see objects inside it.

```sql
GRANT USAGE ON SCHEMA app_data TO read_only_staff;
```

**Granting Select**:

```sql
GRANT SELECT ON TABLE app_data.users TO read_only_staff;
```

**Granting All**:

```sql
GRANT ALL PRIVILEGES ON TABLE app_data.users TO app_admin;
```

**Revoking**:

```sql
REVOKE DELETE ON TABLE app_data.users FROM app_admin;
```

## 3. The "New Table" Problem (Default Privileges)

Common Issue: You grant `SELECT` on all tables to Alice. The next day, you create a _new_ table. Alice cannot see it.
**Reason**: `GRANT` only affects _existing_ objects.

**Solution: ALTER DEFAULT PRIVILEGES**
You can tell Postgres: "Whenever user X creates a table, automatically grant SELECT to user Y".

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA app_data
GRANT SELECT ON TABLES TO read_only_staff;
```

_Note_: This only applies to future tables. You still need to run a one-time GRANT for existing tables.

## 4. Role Attributes

Roles can have special powers:

- `SUPERUSER`: Can do absolutely everything (bypasses all permission checks). **Use Sparingly**.
- `CREATEDB`: Can create database containers.
- `CREATEROLE`: Can create other users.

```sql
ALTER ROLE alice WITH CREATEDB;
```

## Key Takeaways

- **Groups simplify management**: Grant permissions to the `read_only` role, then just add users to that role.
- **Usage vs Select**: You need `USAGE` on the schema before you can `SELECT` from its tables.
- **Default Privileges**: Crucial for avoiding "Permission Denied" errors on newly created tables.
