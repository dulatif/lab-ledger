# 5. Introduction to Resolvers and Object Types

## 🎯 Learning Goal

Map the SDL to TypeScript.

## 🧠 Concept

We have the Schema. Now we need the Logic.
We still use `@Resolver()`.
But we don't pass a class to it (usually).

## 💻 Implementation

### 1. Generate Definitions (Optional but recommended)

Configure `definitions` in `AppModule` to auto-generate TS interfaces.

```typescript
definitions: {
  path: join(process.cwd(), 'src/graphql.ts'),
},
```

### 2. The Resolver

```typescript
@Resolver("Coffee") // Match type name
export class CoffeesResolver {
  @Query() // Match query name 'coffees' autodetected
  async coffees() {
    return [];
  }
}
```

## 🧩 Activity / Challenge

1.  Enable auto-generation.
2.  See `src/graphql.ts` appear.
3.  Use the `Coffee` interface from there.

## 🔑 Key Takeaways

- This flow reduces context switching errors.
