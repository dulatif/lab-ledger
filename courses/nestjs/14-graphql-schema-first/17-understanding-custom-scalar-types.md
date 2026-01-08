# 17. Understanding Custom Scalar Types

## 🎯 Learning Goal

Add custom scalars to SDL.

## 🧠 Concept

1.  Define in SDL: `scalar Date`.
2.  Register in Resolvers map.

## 💻 Implementation

NestJS handles the map registration via `@Scalar` decorator class.

```graphql
scalar Date
```

```typescript
@Scalar('Date')
export class DateScalar { ... }
```

## 🧩 Activity / Challenge

1.  Add `scalar Date` to `schema.graphql`.
2.  Implement the Scalar class.
3.  Use it in `Coffee` type.

## 🔑 Key Takeaways

- The name string `'Date'` must match the SDL scalar name.
