# 9. Conditional Types

## 🎯 Learning Goal

Write "if statements" for types.

## 🧠 Concept

Syntax: `T extends U ? X : Y`
(Does T look like U? If yes, type X, else type Y).

## 💻 Implementation

```typescript
type Message<T> = T extends { message: unknown } ? T["message"] : never;

interface Email {
  message: string;
}
interface Cat {
  meow: string;
}

type EmailMessage = Message<Email>; // string
type CatMessage = Message<Cat>; // never
```

## 🧩 Activity / Challenge

1.  Create `IsArray<T>` type that returns `true` (literal) or `false` (literal).
2.  Test with `IsArray<string[]>` vs `IsArray<number>`.

## 🔑 Key Takeaways

- The foundation of all advanced TS magic.
