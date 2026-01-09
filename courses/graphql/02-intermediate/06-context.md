# Lesson 6: Context & Authentication

## Learning Goals

- The `context` object.
- Extracting the User.

## 1. Setup

Context is created _once_ per Request.
It's usually where you verify the token (JWT).

```javascript
const server = new ApolloServer({
  context: ({ req }) => {
    const token = req.headers.authorization;
    const user = verifyToken(token);
    return { user }; // { user: null } if generic or { user: {...} }
  },
});
```

## 2. Usage

```javascript
createPost: (parent, args, context) => {
  if (!context.user) throw new Error("Not Authenticated");
  return db.createPost(args.title, context.user.id);
};
```

## Key Takeaways

- Authentication happens in `context` creation.
- Authorization happens in the Resolver.
