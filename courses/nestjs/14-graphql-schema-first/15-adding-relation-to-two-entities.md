# 15. Adding Relation to two Entities

## 🎯 Learning Goal

Relations in SDL.

## 🧠 Concept

SDL: `flavors` is an array of `Flavor` objects.

```graphql
type Flavor {
  id: ID!
  name: String!
}

type Coffee {
  # ...
  flavors: [Flavor!]!
}
```

## 💻 Implementation

1.  Add `Flavor` type to SDL.
2.  Update `Coffee` Entity (TypeORM) with `@ManyToMany` (same as before).
3.  In Resolver, you might need a Field Resolver if the data isn't joined automatically.

## 🧩 Activity / Challenge

1.  Model the relation in SDL.
2.  Update `Coffee` entity.
3.  Check if TypeORM `eager: true` makes it work automatically without a Field Resolver.

## 🔑 Key Takeaways

- SDL is just the contract. How you fulfill it (Join vs Lazy Load) is key.
