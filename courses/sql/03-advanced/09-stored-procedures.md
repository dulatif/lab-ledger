# 9. Stored Procedures and Functions

## 🎯 Learning Goal

Logic in the DB.

## 🧠 Concept

Save code on the server.
**Function**: RETURNs a value (Use in SQL).
**Procedure**: Can perform actions/transactions (Call specifically).

## 💻 Implementation

```sql
CREATE FUNCTION add_tax(price DECIMAL) RETURNS DECIMAL AS $$
BEGIN
  RETURN price * 1.1;
END;
$$ LANGUAGE plpgsql;
```

## 🧩 Activity / Challenge

1.  Pros/Cons:
    - Pro: Performance (One network call).
    - Con: Hard to debug/version control compared to App Code.

## 🔑 Key Takeaways

- Use sparingly. Business logic belongs in the App usually.
