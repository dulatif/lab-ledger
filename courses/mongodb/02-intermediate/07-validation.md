# Lesson 7: Schema Validation

## Learning Goals

- JSON Schema.
- Enforcing Rules.

## 1. Why Validation?

MongoDB is flexible, but applications need structure.
Prevent garbage data (`age: "Twenty"`).

## 2. Defining Rules

Set during collection creation or using `collMod`.

```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required",
        },
        age: {
          bsonType: "int",
          minimum: 18,
        },
      },
    },
  },
});
```

## Key Takeaways

- Validation happens on Write (Insert/Update).
- Useful as a "Database Firewall".
