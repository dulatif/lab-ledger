# Lesson 3: Advanced Security

## Learning Goals

- Implement Row-Level Security (RLS) for multi-tenant apps.
- Understand data-in-transit encryption (SSL/TLS).
- Explore Data Anonymization concepts.

## 1. Row-Level Security (RLS)

Standard `GRANT` is "All or Nothing" for a table. RLS allows you to define policies: "User X can only see rows where `tenant_id = X`."

**Enabling RLS**:

```sql
ALTER TABLE app_data.orders ENABLE ROW LEVEL SECURITY;
```

Once enabled, if no policy exists, the table appears **empty** to everyone except the table owner/superuser.

**Creating a Policy**:
Scenario: A SaaS app where users should only see their own orders.

1.  **Define the Policy**:

    ```sql
    CREATE POLICY user_sees_own_orders ON app_data.orders
    FOR SELECT
    USING (user_id = current_user_id()); -- Custom function getting ID from session context
    ```

2.  **Setting Session Variables**:
    Web apps connect as a generic `app_user` (one DB role), but act on behalf of many end users. You can pass the end-user's ID to the DB session:
    ```sql
    SET app.current_user_id = '123';
    SELECT * FROM app_data.orders; -- Automatically filters for user 123
    ```

## 2. SSL/TLS (Encryption in Transit)

To prevent packet sniffing (someone reading your password/data on the wifi), you must force SSL.

**Server Side**:
In `postgresql.conf`:

```conf
ssl = on
ssl_cert_file = 'server.crt'
ssl_key_file = 'server.key'
```

**Client Side (`pg_hba.conf`)**:
Use `hostssl` instead of `host` to _mandate_ encryption.

```conf
# TYPE      DATABASE    USER    ADDRESS     METHOD
hostssl     all         all     0.0.0.0/0   scram-sha-256
```

If a client tries to connect without SSL support, the connection is rejected.

## 3. Data Anonymization

For GDPR/compliance, developers often shouldn't see real production data (emails, credit cards).

**PostgreSQL Anonymizer Extension**:
A popular extension (`postgresql_anonymizer`) allows you to define masking rules.

```sql
SECURITY LABEL FOR anon ON COLUMN users.email
IS 'MASKED WITH FUNCTION partial_email(email)';
```

When a developer queries this (via a specific masked role), they see `a***@example.com` instead of the real email.

## Key Takeaways

- **RLS** moves authorization logic from the Application code into the Database kernel. This is safer because it applies to _all_ access methods (SQL, Reporting tools, etc.).
- **SSL** should be mandatory for any production database not running on `localhost`.
- **Least Privilege**: Don't give developers access to raw production PII (Personally Identifiable Information).
