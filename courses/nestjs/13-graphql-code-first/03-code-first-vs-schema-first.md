# 3. Code First vs Schema First

## 🎯 Learning Goal

Choose your weapon.

## 🧠 Concept

**Schema First**:

1.  Write `schema.graphql` (SDL).
2.  Generate TypeScript interfaces from it.
3.  Write Resolvers affecting those interfaces.

- _Pros_: Language agnostic schema.
- _Cons_: Context switching between SDL and TS.

**Code First (NestJS Preferred)**:

1.  Write TypeScript Classes (`@ObjectType`).
2.  NestJS generates the `schema.graphql` file for you automatically.

- _Pros_: Keep everything in TypeScript. Decorators are powerful.
- _Cons_: NestJS specific.

## 💻 Implementation

We will use **Code First**.

## 🧩 Activity / Challenge

1.  We will stick to TypeScript classes decorated with metadata.
2.  NestJS is heavily opinionated towards Code First (it feels like TypeORM).

## 🔑 Key Takeaways

- Code First reduces duplication (DRY). Your DTO is your Schema.
