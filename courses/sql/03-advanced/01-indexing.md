# 1. Introduction to Indexing

## 🎯 Learning Goal

The #1 way to speed up reads.

## 🧠 Concept

An **Index** is like a Book Index. Instead of scanning every page (Full Table Scan), look up the key and jump to the page.

- **B-Tree**: Default. Good for ranges (`<`, `>`, `=`).
- **Hash**: Good for equality (`=`).

## 💻 Implementation

```sql
CREATE INDEX idx_users_email ON users(email);
```

## 🧩 Activity / Challenge

1.  Trade-off: Indexes speed up READs but slow down WRITEs (INSERT/UPDATE), because the index must be updated too.

## 🔑 Key Takeaways

- Index columns used in WHERE, JOIN, and ORDER BY.
