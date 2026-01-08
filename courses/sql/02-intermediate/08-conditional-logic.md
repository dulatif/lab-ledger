# 8. Conditional Logic (CASE, COALESCE)

## 🎯 Learning Goal

If-Else in SQL.

## 🧠 Concept

**CASE**:

```sql
CASE
  WHEN age < 18 THEN 'Minor'
  ELSE 'Adult'
END
```

**COALESCE(val1, val2)**: Return first non-null value. Great for default values.
`COALESCE(phone, 'No Phone')`.

## 💻 Implementation

```sql
SELECT name,
  CASE WHEN subscription_active THEN 'Premium' ELSE 'Free' END as plan
FROM users;
```

## 🧩 Activity / Challenge

1.  Replace NULL prices with 0.
    - `SELECT COALESCE(price, 0) ...`

## 🔑 Key Takeaways

- Moves logic from App Code to Query.
