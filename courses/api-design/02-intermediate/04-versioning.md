# 4. Versioning Strategies

## 🎯 Learning Goal

Changing the API without breaking clients.

## 🧠 Concept

Breaking Change = Changing response shape/req fields. Requires Versioning.

1.  **URI Path (Best)**: `/v1/users`.
2.  **Header**: `Accept: application/vnd.myapi.v1+json`.
3.  **Query Param**: `?v=1` (Avoid).

## 💻 Implementation

When you need to rename `username` to `userId`:

1.  Create `/v2/users`.
2.  Keep `/v1/users` working (Adapter pattern).
3.  Announce deprecation.

## 🧩 Activity / Challenge

1.  Discuss: Why path versioning (`/v1`) is easiest for developers? (You can browse it easily).

## 🔑 Key Takeaways

- Never break v1. Launch v2.
