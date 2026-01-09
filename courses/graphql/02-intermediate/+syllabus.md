# GraphQL Intermediate Course Syllabus

## Module 5: Advanced Schema Design

**Goal**: Handle complex data structures.

1.  **Enums**
    - Concepts: Restricted set of values.
    - Goal: Model status fields (Active, Inactive, Banned).
2.  **Interfaces & Unions**
    - Concepts: Polymorphism in GraphQL.
    - Goal: Return different types in a single list.
3.  **Directives**
    - Concepts: `@deprecated`, Custom directives.
    - Goal: Modify schema behavior.

## Module 6: Execution & Resolvers

**Goal**: Understand how the server answers queries.

4.  **Execution Flow**
    - Concepts: Root Fields, Parallel execution.
    - Goal: Trace a query from request to response.
5.  **Writing Resolvers**
    - Concepts: The Resolver Signature `(parent, args, context, info)`.
    - Goal: Connect a schema field to a database.
6.  **Context & Authentication**
    - Concepts: Passing User info to every resolver.
    - Goal: Secure the API.

## Module 7: Network & Transport

**Goal**: Serve GQL over HTTP.

7.  **GraphQL over HTTP**
    - Concepts: POST requests, GET (for caching), Status Codes (Always 200?).
    - Goal: Build a compliant server.
8.  **Caching Strategies**
    - Concepts: Client-side caching (Normalized Cache) vs Server-side (Dataloader).
    - Goal: Fix the "N+1 Problem".
