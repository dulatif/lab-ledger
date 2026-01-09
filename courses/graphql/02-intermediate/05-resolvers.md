# Lesson 5: Writing Resolvers

## Learning Goals

- The 4 Arguments.
- Fetching data.

## 1. The Signature

Every resolver function receives 4 arguments:
`resolver(parent, args, context, info)`

1.  **parent**: The result of the previous resolver.
2.  **args**: The arguments provided to the field in the query.
3.  **context**: A shared object for every resolver (Authentication, DB connection).
4.  **info**: AST details (advanced).

## 2. Example

```javascript
const resolvers = {
  Query: {
    user: (parent, args, context) => {
      return context.db.getUser(args.id);
    },
  },
  User: {
    posts: (parent, args, context) => {
      // parent is the User object from above
      return context.db.getPostsByUserId(parent.id);
    },
  },
};
```

## Key Takeaways

- Resolvers connect the Schema to the Data.
- Keep resolvers thin. Put business logic in Model/Service functions.
