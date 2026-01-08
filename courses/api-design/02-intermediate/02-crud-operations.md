# 2. Handling CRUD Operations

## 🎯 Learning Goal

Map Database actions to HTTP.

## 🧠 Concept

- **Create**: `POST /items`. Returns `201 Created` + `Location` header.
- **Read**: `GET /items` or `GET /items/1`. Returns `200 OK`.
- **Update**: `PUT /items/1`. Returns `200` or `204`.
- **Delete**: `DELETE /items/1`. Returns `204 No Content`.

## 💻 Implementation

Consistency is key.
If DELETE returns 200 today and 204 tomorrow, clients break.

## 🧩 Activity / Challenge

1.  What if the ID doesn't exist?
    - GET/PUT/DELETE -> 404 Not Found.
    - POST -> 409 Conflict (if ID provided and duplicates).

## 🔑 Key Takeaways

- Don't hide errors. If it failed, return 4xx/5xx.
