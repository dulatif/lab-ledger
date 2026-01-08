# 6. GraphQL Schemas, Types, and Scalars

## 🎯 Learning Goal

Understand the generated schema.

## 🧠 Concept

Look at the generated `src/schema.gql`.

```graphql
type Coffee {
  id: Int!
  name: String!
  brand: String!
}

type Query {
  coffees: [Coffee!]!
}
```

**Scalars**:

- `String`, `Int`, `Float`, `Boolean`, `ID`.
- NestJS maps TS `string` -> GraphQL `String`.
- NestJS maps TS `number` -> GraphQL `Float` (Default).
- Use `@Field(type => Int)` to force Integer.

## 💻 Implementation

Open `http://localhost:3000/graphql`.
This is the **Playground**.
Run:

```graphql
query {
  coffees {
    name
  }
}
```

## 🧩 Activity / Challenge

1.  Play with the Playground.
2.  Add a `flavors: string[]` to the entity.
3.  See valid schema change.

## 🔑 Key Takeaways

- The Playground is your best friend (Self-documenting API).
