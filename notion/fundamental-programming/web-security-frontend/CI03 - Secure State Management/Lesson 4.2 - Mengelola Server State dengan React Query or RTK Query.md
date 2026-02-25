💡 CI03.4.2: The Right Tool for the Job: Managing Server State with React Query

**Outline:**

- **The Threat (SEEI):** Manually implementing the complex logic for fetching, caching, synchronizing, and updating server data using `useEffect` and `useState`. This approach is verbose, error-prone, and reinvents the wheel.
- **The Defense (PPP):** The principle of "Declarative Data Fetching." Using a dedicated server-state library like React Query to abstract away the complexity. You tell it _what_ data you need, and it handles _how_ and _when_ to fetch it.
- **Your Mission (Production):** Refactor a component that fetches data with `useEffect` to use the `useQuery` hook from React Query.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **imperative complexity**. When you fetch data manually, you are responsible for managing every part of the process.

```ts
// VULNERABLE (MANUAL & COMPLEX) CODE
function PostList() {
  const [posts, setPosts] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setIsLoading(true);
    fetch('/api/posts')
      .then(res => res.json())
      .then(data => {
        setPosts(data);
        setIsLoading(false);
      })
      .catch(err => {
        setError(err);
        setIsLoading(false);
      });
  }, []); // The empty dependency array is a bug! This will never re-fetch.

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;

  // ... render posts
}

```

This code has many problems:

- It's a lot of boilerplate for one simple fetch.
- It doesn't handle re-fetching when the user navigates back to the page.
- It doesn't share data; another component needing posts would have to fetch them again.
- It doesn't handle caching or background updates.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Don't reinvent the data-fetching wheel. Use a dedicated library."** Tools like React Query (or RTK Query, SWR, Apollo) are experts at managing server cache. They provide simple hooks that handle all the complexity for you.

**Secure Code Implementation (Using React Query):**

First, install React Query and set up the `QueryClientProvider` in your app's root. `npm install @tanstack/react-query`

Now, let's refactor our component.

```ts
// SECURE (DECLARATIVE & SIMPLE) CODE
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

// Define the fetcher function once
const fetchPosts = async () => {
  const { data } = await axios.get('/api/posts');
  return data;
};

function PostList() {
  // The useQuery hook handles everything for us!
  const { data: posts, isLoading, isError } = useQuery({
    queryKey: ['posts'], // A unique key for this data
    queryFn: fetchPosts,  // The function to fetch the data
  });

  if (isLoading) return <p>Loading...</p>;
  if (isError) return <p>Error!</p>;

  // ... render posts
}

```

Look at everything we get for free with this change:

- **Caching:** If another component calls `useQuery` with `queryKey: ['posts']`, it will get the cached data instantly without a new network request.
- **Automatic Re-fetching:** React Query automatically re-fetches data when the user re-focuses the window, ensuring the UI is always fresh.
- **Simplified State:** No more manual `useState` for `isLoading` or `error`. The hook provides these for us.
- **DevTools:** React Query comes with amazing developer tools for inspecting your cache.

### 🤔 Reflective Questions

1. The `queryKey` in `useQuery` is crucial. If you have a query that fetches a specific post (e.g., `/api/posts/123`), what would be an appropriate `queryKey`? Why is `['posts']` not specific enough?
2. How does using a library like React Query for server state simplify the data you need to store in your global Redux/Zustand store?

### 📚 Further Reading

- **React Query:** [Official Documentation](https://tanstack.com/query/latest) - The docs are some of the best in the open-source world.

### 📝 Mini Task (Production)

You need to build a "User Profile" component that fetches data from `/api/users/:userId`. The `userId` is passed to the component as a prop.

Your task: Write the `useQuery` hook call for this component. Pay close attention to how you would structure the `queryKey` to make it unique for each user.