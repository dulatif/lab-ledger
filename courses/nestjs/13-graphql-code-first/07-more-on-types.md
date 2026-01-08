# 7. More on Types

## 🎯 Learning Goal

Handle Nullability and Arrays.

## 🧠 Concept

In GraphQL, fields are **Non-Nullable** by default (`String!`).
In Typescript, fields are Non-Nullable by default.

**To make nullable**:
`@Field({ nullable: true })`
Generates: `name: String` (No exclamation mark).

**Arrays**:
`@Field(type => [String])`
Generates: `flavors: [String!]!`

## 💻 Implementation

```typescript
@Field({ nullable: true })
description?: string; // Optional in GQL and TS
```

## 🧩 Activity / Challenge

1.  Make `brand` nullable.
2.  Check generated schema.
3.  Try to query it.

## 🔑 Key Takeaways

- Be explicit about nullability. It communicates to the Frontend "Hey, this might be missing, handle it".
