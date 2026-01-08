# 3. Pagination, Filtering, and Sorting

## 🎯 Learning Goal

Managing large datasets.

## 🧠 Concept

Never return `SELECT *` without limits.

- **Pagination**: `?page=1&limit=10` or Cursor-based `?after=cursor_id`.
- **Filtering**: `?category=electronics&price_lt=500`.
- **Sorting**: `?sort=-created_at` (Descending).

## 💻 Implementation

Response Envelope:

```json
{
  "data": [...],
  "meta": {
    "total": 100,
    "page": 1,
    "has_next": true
  }
}
```

## 🧩 Activity / Challenge

1.  Why is Offset pagination (`skip/limit`) bad for large tables? (Performance degrades).
2.  Why is Cursor pagination better? (Use Index seeking).

## 🔑 Key Takeaways

- Design pagination from Day 1. It's hard to add later.
