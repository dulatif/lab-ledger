# 16. Function and Constructor Overloading

## 🎯 Learning Goal

Define multiple call signatures for one function.

## 🧠 Concept

JS allows flexible arguments. TS models this with Overloads.
You write **Overload Signatures** (types) and one **Implementation Signature** (logic).

## 💻 Implementation

```typescript
// Overload 1
function makeDate(timestamp: number): Date;
// Overload 2
function makeDate(m: number, d: number, y: number): Date;

// Implementation (Not visible to caller)
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}

makeDate(12345678); // OK
makeDate(5, 5, 2025); // OK
// makeDate(1, 2); // Error (Matched neither overload)
```

## 🧩 Activity / Challenge

1.  Overload a `getElement` function.
2.  Accept string (ID) or number (Index).

## 🔑 Key Takeaways

- The implementation signature must be compatible with ALL overloads.
- Only the overloads are callable.
