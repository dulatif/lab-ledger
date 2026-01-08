# 2. Query Analysis and Explain Plans

## 🎯 Learning Goal

Looking under the hood.

## 🧠 Concept

**EXPLAIN**: Asks database "How will you run this?".
Shows if it uses an Index (Seek/Scan) or reads the whole table (Seq Scan).

## 💻 Implementation

```sql
EXPLAIN SELECT * FROM users WHERE email = 'bob@example.com';
```

## 🧩 Activity / Challenge

1.  Run EXPLAIN before and after adding an index.
2.  Look for "Cost" drop.

## 🔑 Key Takeaways

- Never guess performance. Measure it.
