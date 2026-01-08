# 1. URI Design and Resource Modeling

## 🎯 Learning Goal

Design clean, intuitive paths.

## 🧠 Concept

- **Nouns, not Verbs**: `/getUsers` (Bad) vs `/users` (Good).
- **Plurals**: `/users` is standard convention.
- **Hierarchy**: `/users/1/posts` (Posts belonging to User 1).

## 💻 Implementation

- **Collection**: `GET /products`
- **Singleton**: `GET /products/123`
- **Sub-resource**: `GET /orders/10/items`

## 🧩 Activity / Challenge

1.  Model a School system.
2.  `Students`, `Courses`, `Grades`.
3.  Design URI for "Get all grades for Student X in Course Y".
    - `/students/X/courses/Y/grades` or `/grades?student=X&course=Y`. Both valid.

## 🔑 Key Takeaways

- The URI should be guessable.
