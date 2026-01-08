# 9. Adding Indexes to Schemas

## 🎯 Learning Goal

Speed up queries.

## 🧠 Concept

If you search by `name`, Mongo scans every document (Collection Scan). Slow. 🐢
If you add an index, it uses a B-Tree. Fast. 🐇

## 💻 Implementation

### 1. Single Field

```typescript
@Prop({ index: true })
name: string;
```

### 2. Compound Index (Decorate Class)

```typescript
@Schema()
@index({ name: 1, brand: -1 }) // Compound index
export class Coffee ...
```

## 🧩 Activity / Challenge

1.  Enable query logging in Mongo (or just trust me).
2.  Add `@Prop({ unique: true })` to name.
3.  Try to create two coffees with the same name.
4.  It throws a crucial error: `E11000 duplicate key error`.

## 🔑 Key Takeaways

- Indexes are for Performance AND Data Integrity (Uniqueness).
