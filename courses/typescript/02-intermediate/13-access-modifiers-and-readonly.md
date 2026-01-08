# 13. Access Modifiers and readonly

## 🎯 Learning Goal

Control visibility and mutability.

## 🧠 Concept

- `public` (Default): Accessible everywhere.
- `private`: Accessible only within the class. (Soft private, still present in JS runtime).
- `protected`: Accessible within class and subclasses.
- `readonly`: Cannot assign to property outside constructor.

## 💻 Implementation

```typescript
class Database {
  private connectionString: string;
  readonly dbName: string = "Prod";

  constructor(conn: string) {
    this.connectionString = conn;
  }
}
```

### JS Private Fields (#)

Typescript `private` is erased at runtime.
JS `#private` (Hard private) is enforced by the engine.

```typescript
class Secret {
  #code = 123; // Runtime private
}
```

## 🧩 Activity / Challenge

1.  Create a `BankAccount` class.
2.  Make `balance` private.
3.  Expose `deposit()` and `withdraw()` public methods.

## 🔑 Key Takeaways

- Use `private` for API boundaries within your TS code.
- Use `#` if you are writing a library and need hard runtime privacy.
