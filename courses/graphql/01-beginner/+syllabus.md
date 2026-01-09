# GraphQL Beginner Course Syllabus

## Module 1: Core Concepts

**Goal**: Understand the philosophy and mental model.

1.  **What is GraphQL?**
    - Concepts: Declarative data fetching, Single Endpoint, Typed Schema.
    - Goal: Compare REST vs GraphQL ("Over-fetching" & "Under-fetching").
2.  **Thinking in Graphs**
    - Concepts: Nodes, Edges, Graphs.
    - Goal: Visualize data as a graph relationships.

## Module 2: Fetching Data (Queries)

**Goal**: Learn to ask for exactly what you need.

3.  **Basic Queries**
    - Concepts: Use `query`, Selection Sets, Fields.
    - Goal: Write your first query.
4.  **Arguments & Variables**
    - Concepts: Filtering data, Dynamic inputs (`$id`).
    - Goal: Fetch a specific user by ID.
5.  **Aliases & Fragments**
    - Concepts: Renaming fields, Reusing logic.
    - Goal: Avoid repetition in queries.

## Module 3: Modifying Data (Mutations)

**Goal**: Learn to change server-side data.

6.  **Basic Mutations**
    - Concepts: `mutation` keyword.
    - Goal: Create a new resource.
7.  **Input Types**
    - Concepts: Passing objects as arguments.
    - Goal: Clean up complex mutation signatures.

## Module 4: The Type System

**Goal**: Define the Shape of your API.

8.  **Scalars & Objects**
    - Concepts: `Int`, `String`, `Boolean`, `ID`, Custom Objects.
    - Goal: Define a User type.
9.  **Lists & Non-Null**
    - Concepts: `[Type]`, `Type!`.
    - Goal: Model mandatory fields and arrays.
