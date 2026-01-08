# 2. Introduction to GraphQL with NestJS

## 🎯 Learning Goal

Setup Apollo Driver for Schema First.

## 🧠 Concept

Configuration is slightly different.
Instead of `autoSchemaFile` (Output), we use `typePaths` (Input).

## 💻 Implementation

```typescript
GraphQLModule.forRoot<ApolloDriverConfig>({
  driver: ApolloDriver,
  typePaths: ["./**/*.graphql"], // WHERE are the files?
});
```

## 🧩 Activity / Challenge

1.  Configure `AppModule`.
2.  Run the app. It will crash (No files found yet).

## 🔑 Key Takeaways

- NestJS scans your project for `.graphql` files and combines them into one schema.
