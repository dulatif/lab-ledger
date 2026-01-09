# Lesson 3: Basic Queries

## Learning Goals

- The `query` keyword.
- Selection Sets.

## 1. Structure

Every GraphQL operation starts with a root type (usually `query`, `mutation`, or `subscription`).

```graphql
query GetMyData {
  # Field
  hero {
    # Selection Set
    name
    height
  }
}
```

## 2. Fields

Fields are keys on an object.
Scalar fields (like `name` or `height`) terminate the query.
Object fields (like `hero`) require a sub-selection.

**Invalid**:

```graphql
query {
  hero # Error! Hero is an object, you must select fields inside it.
}
```

## Key Takeaways

- The shape of the Response matches the shape of the Query.
- `query` is the default operation (you can omit the keyword if it's the only op), but naming them is best practice.
