# Lesson 6: Typing Objects

## Learning Goals

- Inline Types.
- `type` vs `interface`.

## 1. Inline

```typescript
function printUser(user: { name: string; age: number }) { ... }
```

Hard to read.

## 2. Type Alias

```typescript
type User = {
  name: string;
  age: number;
  email?: string; // Optional
};
```

## 3. Interface

```typescript
interface User {
  name: string;
  age: number;
}
```

## Key Takeaways

- Use `interface` for public APIs (can be merged).
- Use `type` for unions/intersections and complex logic.
- Don't overthink it, pick one and be consistent.
