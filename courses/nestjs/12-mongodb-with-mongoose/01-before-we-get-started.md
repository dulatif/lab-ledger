# 1. Before we Get Started

## 🎯 Learning Goal

Understand why MongoDB is a popular choice for Node.js APIs.

## 🧠 Concept

- **NoSQL**: Flexible schema. Great for rapid prototyping or data with variable structures.
- **JSON-Native**: MongoDB stores BSON (Binary JSON), which maps 1:1 with JavaScript Objects.
- **Mongoose**: The ODM (Object Document Mapper) that adds Schema validation back to the schemaless DB.

## 💻 Implementation

We will build a simple "Coffees" application (classic NestJS example) but using Mongo instead of Postgres.

## 🧩 Activity / Challenge

1.  Forget about "Tables" and "Rows".
2.  Think in "Collections" and "Documents".
3.  Think about "Embedding" data (Comments inside a Post) vs "Referencing" data (Comment ID inside a Post).

## 🔑 Key Takeaways

- MongoDB allows flexibility, but Mongoose keeps us sane with types.
