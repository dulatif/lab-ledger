# 8. Introduction to Domain-Driven Design

## 🎯 Learning Goal

Learn the basic building blocks of DDD: **Entities**, **Value Objects**, and **Aggregates**.

## 🧠 Concept

**DDD** is a set of tools to model complex software. It tells us how to design our classes.

### 1. Entity

An object defined by its **Identity** (ID).

- Example: A `User`. If I change my email, I am still the same User (ID=1).

### 2. Value Object

An object defined by its **Attributes**. No ID. Immutable.

- Example: A `Color(red, green, blue)`. If I change 'red' to 255, it is a _different_ color.
- Example: `Address(street, city)`. Two addresses are the same if the street and city are the same.

### 3. Aggregate

A cluster of objects treated as a unit for data changes.

- Example: An `Order` (Aggregate Root) contains `OrderItems` (Entities).
- **Rule**: You cannot access `OrderItem` directly. You must go through the `Order`. `order.addItem(...)`.

## 💻 Implementation

```typescript
// Value Object
class Address {
  constructor(public readonly city: string) {}

  equals(other: Address): boolean {
    return this.city === other.city;
  }
}

// Entity & Aggregate Root
class User extends AggregateRoot {
  constructor(private readonly id: string) {
    super();
  }

  private address: Address;

  moveTo(newCity: string) {
    this.address = new Address(newCity); // Logic happens here
  }
}
```

## 🧩 Activity / Challenge

1.  Think about an e-commerce app.
2.  Identification: Is a `CreditCard` an Entity or Value Object? (Debatable! Usually Value Object in payment contexts).
3.  Is `ShoppingCart` an Aggregate? (Yes).

## 🔑 Key Takeaways

- **Encapsulation**: Aggregates protect their internal state. No `setItems()`. Only `addItem()`.
- **Consistency**: Transactions usually happen at the Aggregate level.
