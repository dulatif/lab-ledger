# Lesson 2: Interfaces & Unions

## Learning Goals

- Polymorphism.
- `interface` vs `union`.

## 1. Interfaces

A blueprint. Types "implement" the interface.

```graphql
interface Character {
  id: ID!
  name: String!
}

type Human implements Character {
  id: ID!
  name: String!
  starships: [Starship]
}

type Droid implements Character {
  id: ID!
  name: String!
  primaryFunction: String
}
```

## 2. Unions

A collection of types that don't necessarily share fields.

```graphql
union SearchResult = Human | Droid | Starship
```

Querying a Union:

```graphql
query Search {
  search(text: "R2") {
    ... on Human {
      name
    }
    ... on Droid {
      primaryFunction
    }
    ... on Starship {
      model
    }
  }
}
```

## Key Takeaways

- Use **Interfaces** when types share common fields.
- Use **Unions** when returning arbitrary mixed types (like specific Search results).
