# Lesson 1: What is GraphQL?

## Learning Goals

- The Definition.
- Single Endpoint.
- Over-fetching & Under-fetching.

## 1. The Definition

GraphQL is a **Query Language for your API** and a runtime for fulfilling those queries with your existing data.
It is _not_ a database. It sits in front of your database.

## 2. REST vs GraphQL

**REST**:

- `/users/1` -> Returns ALL user data.
- `/users/1/posts` -> Returns posts.
- **Problem 1 (Over-fetching)**: You only needed the user's name, but you got address, birthday, etc.
- **Problem 2 (Under-fetching)**: You need the user + their last 3 posts. You must make 2 requests.

**GraphQL**:

- One endpoint: `/graphql`.
- You ask for exactly what you want:

```graphql
query {
  user(id: "1") {
    name
    posts(limit: 3) {
      title
    }
  }
}
```

- You get exactly that JSON back. 1 Request.

## Key Takeaways

- GraphQL pushes the control of data fetching from the Server to the Client.
- It solves network efficiency problems.
