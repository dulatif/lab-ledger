# Lesson 7: Networking & Fetch

## Learning Goals

- Fetch data from APIs.
- Handle formatting and loading states.
- Use third-party libraries (Axios).

## 1. The Fetch API

React Native ships with the standard Web `fetch` API.

```jsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);

const getUsers = async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    const json = await response.json();
    setData(json);
  } catch (error) {
    console.error(error);
  } finally {
    setLoading(false);
  }
};

useEffect(() => {
  getUsers();
}, []);
```

## 2. Using Axios

Many developers prefer `axios` because it automatically transforms JSON and handles errors better (throws on 4xx/5xx).

```bash
npm install axios
```

## 3. Network Images

Remember: To display an image from a URL, the `<Image>` component needs width/height setup.

## Key Takeaways

- **useEffect**: The standard place to trigger initial data loads.
- **Async/Await**: Makes asynchronous code readable.
- **Https**: iOS and Android block cleartext HTTP by default. Ensure your API uses HTTPS.
