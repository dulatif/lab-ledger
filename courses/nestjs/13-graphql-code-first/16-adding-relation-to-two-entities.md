# 16. Adding Relation to two Entities

## 🎯 Learning Goal

Relate `Coffee` to `Flavor`.

## 🧠 Concept

Currently `flavors` is just `string[]`.
Let's make it an Entity `Flavor`.

## 💻 Implementation

1.  Create `Flavor` Entity + ObjectType.
2.  Update `Coffee`:
    ```typescript
    @JoinTable()
    @ManyToMany(type => Flavor, flavor => flavor.coffees, { cascade: true })
    @Field(type => [Flavor]) // GraphQL
    flavors: Flavor[];
    ```

## 🧩 Activity / Challenge

1.  Refactor `CreateCoffeeInput` to accept `flavors: string[]`.
2.  Logic: Pre-load flavors by name, create if not exist.
3.  Query Nested data:
    ```graphql
    {
      coffees {
        name
        flavors {
          name
        }
      }
    }
    ```

## 🔑 Key Takeaways

- GraphQL shines here. You can traverse the graph easily.
