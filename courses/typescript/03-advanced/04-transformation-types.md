# 4. Transformation Types (Partial, Pick, Omit, Record)

## 🎯 Learning Goal

Modify existing types.

## 🧠 Concept

TS provides built-in Mapped Types to avoid duplication.

- `Partial<T>`: Makes all properties optional.
- `Pick<T, K>`: Selects specific keys.
- `Omit<T, K>`: Removes specific keys.
- `Record<K, T>`: Creates a map type.

## 💻 Implementation

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

type TodoPreview = Pick<Todo, "title">;
type TodoInfo = Omit<Todo, "title">;
```

## 🧩 Activity / Challenge

1.  Create `User` (id, name, password, email).
2.  Create `UserDTO` using `Omit` to remove password.
3.  Create `UserPatch` using `Partial` on the `UserDTO`.

## 🔑 Key Takeaways

- Always define your "canonical" model first, then derive other versions using utility types.
