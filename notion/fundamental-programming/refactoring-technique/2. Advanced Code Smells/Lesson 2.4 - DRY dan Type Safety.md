# 💡 ACS02.4: DRY dan Type Safety

**Outline:**

- **The Smell (SEEI):** Identifying subtle duplication in logic and types that isn't just copy-paste.
- **The Refactor (PPP):** Applying TypeScript Generics and advanced types to create reusable, type-safe abstractions.
- **Your Refactoring Mission (Production):** Time to unify duplicated logic using generics.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Subtle Duplication (Logical and Structural)**
"Don't Repeat Yourself" (DRY) is a fundamental principle. Beyond copy-pasted code, advanced smells include duplicated _logic_ or _structure_—like two functions performing the same algorithm on different types, or identical interfaces. This indicates a missing reusable abstraction.

**The Negative Impact: The "Update in Two Places" Bug**

- **Maintenance Nightmare:** Changing logic requires updating every duplicate. Forgetting one is a common source of bugs.
- **Increased Code Bloat:** The codebase becomes larger and harder to navigate.
- **Inconsistent Behavior:** Over time, duplicates diverge as developers update one but not the others.
- **Weak Type System:** Duplicated types prevent writing general functions that operate on shared structure.

**Example of the Smell: Duplicated Paging Logic**
Two identical functions for users and products. The algorithm is duplicated.

```ts
interface User {
  id: string;
  name: string;
}
interface Product {
  id: string;
  price: number;
}

const allUsers: User[] = [
  /* ... */
];
const allProducts: Product[] = [
  /* ... */
];

// Duplicated Logic Block 1
function getUsersPage(pageNumber: number, pageSize: number): User[] {
  const startIndex = (pageNumber - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return allUsers.slice(startIndex, endIndex);
}

// Duplicated Logic Block 2
function getProductsPage(pageNumber: number, pageSize: number): Product[] {
  const startIndex = (pageNumber - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return allProducts.slice(startIndex, endIndex);
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Abstract with Generics**
TypeScript's **Generics** allow a function to operate on a variety of types while maintaining full type safety.

1. **Identify the Duplicated Algorithm.**
2. **Create a Generic Function:** Replace specific types with a generic type parameter (e.g., `<T>`).
3. **Define Constraints (if necessary):** Use `extends` if the function needs specific properties.
4. **Replace the Old Functions.**

**Step-by-Step Implementation: Creating a Generic `getPage` function**
Combine `getUsersPage` and `getProductsPage`.

```ts
// Generic function works with an array of ANY type `T`
function getPage<T>(items: T[], pageNumber: number, pageSize: number): T[] {
  const startIndex = (pageNumber - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return items.slice(startIndex, endIndex);
}

// Calls are now fully type-safe with inferred return types
const userPage: User[] = getPage(allUsers, 1, 10);
const productPage: Product[] = getPage(allProducts, 2, 5);
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Duplicated Types and Logic):**
  ```ts
  interface UserApiResponse {
    data: User[];
    error: string | null;
  }
  interface ProductApiResponse {
    data: Product[];
    error: string | null;
  }
  function fetchUsers(): UserApiResponse {
    /* ... */
  }
  ```
- **After (Generic Type and Function):**

  ```ts
  interface ApiResponse<T> {
    data: T;
    error: string | null;
  }

  async function fetchApiData<T>(endpoint: string): Promise<ApiResponse<T>> {
    // ... generic fetch logic ...
  }
  const userResponse = await fetchApiData<User[]>("/users");
  ```

- **The Improvement:**
  - **DRY Principle:** Single source of truth for API structure and fetch logic.
  - **Type Safety:** TypeScript knows `userResponse.data` is `User[]`.
  - **Reduced Code Size:** Replaced multiple types/functions with one reusable generic.

## 🤔 **Reflective Questions**

1. What is a generic constraint (`<T extends SomeType>`) and when do you need it?
2. How can generics be used with classes and interfaces?
3. How do Mapped Types help enforce DRY at the type level?

## 📚 **Further Reading**

- **TypeScript Doc:** Official documentation on [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html).
- **The Bible:** "Form Template Method" is a related pattern for duplicated algorithms.
- **Functional Programming:** Higher-order functions are another way to abstract logic.

## 📝 **Mini Task (Production)**

Refactor the `findPostById` and `findCommentById` functions below into a single, generic `findById` function.

**Your Mission:**

1. Create a generic `findById` function.
2. Use a generic constraint to ensure objects have an `id: string`.
3. Replace the original calls.

```ts
interface Post {
  id: string;
  title: string;
}
interface Comment {
  id: string;
  text: string;
}

const posts: Post[] = [{ id: "a", title: "First Post" }];
const comments: Comment[] = [{ id: "x", text: "Great!" }];

function findPostById(id: string): Post | undefined {
  return posts.find((p) => p.id === id);
}

function findCommentById(id: string): Comment | undefined {
  return comments.find((c) => c.id === id);
}
```
