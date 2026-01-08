# 6. Numeric and Math Functions

## 🎯 Learning Goal

Calculating logic.

## 🧠 Concept

- `ROUND(val, decimals)`: Rounding.
- `FLOOR/CEILING`: Force down/up.
- `ABS`: Absolute value.
- `MOD`: Remainder.

## 💻 Implementation

```sql
SELECT product, ROUND(price * 1.20, 2) as price_with_tax
FROM products;
```

## 🧩 Activity / Challenge

1.  Check if a number is even?
    - `WHERE val % 2 = 0`.

## 🔑 Key Takeaways

- Handle money carefully (Use Decimal types, not Float, to avoid rounding errors).
