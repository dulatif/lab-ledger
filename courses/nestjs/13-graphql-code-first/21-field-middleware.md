# 21. Field Middleware

## 🎯 Learning Goal

Intercept field access (e.g., for logging or permission).

## 🧠 Concept

Like Interceptors, but for specific fields in the Graph.

## 💻 Implementation

```typescript
const loggerMiddleware: FieldMiddleware = async (ctx, next) => {
  const value = await next();
  console.log(value);
  return value;
};

@Field({ middleware: [loggerMiddleware] })
secretField: string;
```

## 🧩 Activity / Challenge

1.  Log access to strictly confidential coffee recipes.

## 🔑 Key Takeaways

- Powerful for fine-grained authorization.
