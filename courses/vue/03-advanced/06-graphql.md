# Lesson 6: GraphQL Integration

## Learning Goals

- Apollo Client (`@vue/apollo-composable`).
- Query vs Mutation.

## 1. Setup

Install `graphql-tag` and access the client.

```typescript
import { useQuery } from "@vue/apollo-composable";
import gql from "graphql-tag";

const { result } = useQuery(gql`
  query getUsers {
    users {
      id
      name
    }
  }
`);
```

## 2. Reactive Variables

Apollo caches normalized data. If you fetch User:1 in one place, and update User:1 in another, the UI updates everywhere automatically.

## Key Takeaways

- GraphQL prevents over-fetching (getting too much data) and under-fetching.
- Vue's Apollo integration is excellent and component-driven.
