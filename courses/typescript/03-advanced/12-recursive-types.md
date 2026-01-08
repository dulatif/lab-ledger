# 12. Recursive Types

## 🎯 Learning Goal

Type structure that references itself.

## 🧠 Concept

Interfaces and Types can refer to themselves.

## 💻 Implementation

### JSON Type

```typescript
type JSONValue =
  | string
  | number
  | boolean
  | null
  | JSONValue[]
  | { [key: string]: JSONValue };
```

## 🧩 Activity / Challenge

1.  Define a `TreeNode<T>` interface.
2.  It needs a `value: T` and `children: TreeNode<T>[]`.

## 🔑 Key Takeaways

- Use cautiously. Deep recursion can crash the compiler ("Type instantiation is excessively deep").
