# 7. Authentication vs Authorization

## 🎯 Learning Goal

Who are you vs What can you do.

## 🧠 Concept

- **Authentication (AuthN)**: Identifying the user (Login). "I am Alice".
- **Authorization (AuthZ)**: Checking permissions. "Alice can delete Posts".

## 💻 Implementation

1.  User sends Creds -> Server returns Token (AuthN).
2.  User requests Delete -> Server checks Token Scopes (AuthZ).

## 🧩 Activity / Challenge

1.  Analogy:
    - ID Card = AuthN.
    - Keycard Access Levels = AuthZ.

## 🔑 Key Takeaways

- Keep them separate in your logic.
