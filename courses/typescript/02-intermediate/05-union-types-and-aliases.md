# 5. Union Types and Type Aliases

## 🎯 Learning Goal

Handle values that can be "one of many" types.

## 🧠 Concept

**Union (`|`)**: The value is A **OR** B.
TypeScript will only allow you to access members common to BOTH, unless you narrow the type.

## 💻 Implementation

```typescript
type ID = string | number;

function printId(id: ID) {
  // console.log(id.toUpperCase()); // Error! number doesn't have toUpperCase

  if (typeof id === "string") {
    console.log(id.toUpperCase()); // Safe!
  } else {
    console.log(id.toFixed(2)); // Safe! inferred as number
  }
}
```

### Discriminated Unions (Tagged Unions)

The most powerful pattern in TS.

```typescript
interface Success {
  status: "success";
  data: string;
}
interface Error {
  status: "error";
  message: string;
}

type Response = Success | Error;

function handle(res: Response) {
  if (res.status === "success") {
    console.log(res.data); // OK
  } else {
    console.log(res.message); // OK
  }
}
```

## 🧩 Activity / Challenge

1.  Create a `Shape` union (`Circle | Square`).
2.  Add a `kind` literal property to use as a discriminator.
3.  Write a function to calculate area based on the `kind`.

## 🔑 Key Takeaways

- Unions model real-world ambiguity better than inheritance.
- Discriminated Unions are idiomatic Redux/State Management patterns.
