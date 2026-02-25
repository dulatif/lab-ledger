### **💡 AA01-2.3: Server-Side Data Fetching in Action**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Creating dynamic, real-world pages.
- **The Optimization Technique (Practice):** Using `fetch` inside `getServerSideProps`.
- **Your Performance Mission (Production):** Time to build a dynamic, data-driven page.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to build dynamic pages where the content is fetched from an external API on every request, ensuring the data is always fresh, while still delivering great SEO and a fast **Largest Contentful Paint (LCP)**.
- **Identifying the Problem:** Hardcoded data is useful for learning, but real apps need real data. The bottleneck is the round-trip to an external API. By performing this fetch on the server inside `getServerSideProps`, we co-locate our data fetching with our rendering. This is more efficient than a client-side waterfall where the browser has to wait for JS to load, then make a separate request for data.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Since `getServerSideProps` runs in a Node.js environment, we can use standard web APIs like `fetch` to get data from anywhere on the internet. We `await` the response and then pass the resulting data to our component as props.
    
- **Secure Code Implementation:** Let's build a dynamic page that shows details for a specific product. The URL will be `/products/[id]`.
    
    ```tsx
    // file: pages/products/[id].tsx
    import type { GetServerSideProps, NextPage } from 'next';
    
    // Define the shape of our Product data and props
    interface Product {
      id: number;
      title: string;
      description: string;
      price: number;
    }
    interface ProductPageProps {
      product: Product;
    }
    
    // The Page Component
    const ProductPage: NextPage<ProductPageProps> = ({ product }) => {
      // A fallback for when data fetching fails
      if (!product) {
        return <p>Product not found!</p>;
      }
      return (
        <div>
          <h1>{product.title}</h1>
          <p>{product.description}</p>
          <strong>Price: ${product.price}</strong>
        </div>
      );
    };
    
    // The SSR Function
    export const getServerSideProps: GetServerSideProps = async (context) => {
      // Get the product ID from the URL (e.g., /products/1 -> id will be '1')
      const { id } = context.params!;
    
      const res = await fetch(`https://dummyjson.com/products/${id}`);
      const product = await res.json();
    
      // Pass the fetched product data to the page
      return {
        props: {
          product,
        },
      };
    };
    
    export default ProductPage;
    
    ```
    
    Now, if you navigate to `/products/1`, `getServerSideProps` will fetch product #1 from the API, and the server will render the page with that product's title and price. Navigating to `/products/2` will fetch and render product #2.
    

### 🧠 **Real-World Case Study: "The SEO-Invisible E-Commerce Page"**

- **The Scenario:** An e-commerce site wants its product pages to rank high on Google.
- **Before Optimization (CSR):** The page `/product/123` loads an app shell. The client-side JavaScript then reads the ID `123` from the URL and makes a request to `/api/products/123`. While loading, the page title is generic. Google's crawler sees an empty `div` with a loading spinner and may fail to index the product's content properly.
- **After Optimization (SSR):** A request to `/product/123` hits the server. `getServerSideProps` fetches the product data. The server renders the full HTML with the product's name in the `<h1>` and `<title>` tags, a full description, and price. When Google's crawler hits the URL, it receives a complete, content-rich document, perfect for SEO.

### 🤔 **Reflective Questions**

1. How would you handle errors inside `getServerSideProps`, for example, if the `fetch` call fails or returns a 404? (Hint: look up the `notFound` and `redirect` return properties in the Next.js docs).
2. What is the difference between `context.params` and `context.query` in `getServerSideProps`?
3. If you have an API that is slow, how will that affect the user experience of an SSR page? What metric will be most impacted?

### 📚 **Further Reading**

- **Web Vitals:** [Largest Contentful Paint (LCP)](https://web.dev/lcp/)
- **Next.js Docs:** [Data Fetching: `getServerSideProps`](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)
- **Dummy API:** [DummyJSON](https://dummyjson.com/docs/products) for practice data.

### 📝 **Mini Task (Production)**

- **Your Mission:** You are tasked with creating a user profile page that fetches data from a public API.
    1. Create a dynamic route at `pages/users/[id].tsx`.
    2. Use the JSONPlaceholder API endpoint for users: `https://jsonplaceholder.typicode.com/users/[id]`.
    3. In `getServerSideProps`, fetch the user's data based on the `id` from the URL.
    4. The page component should receive the user data as a prop and display the user's name, email, and phone number.
    5. Test your page by navigating to `/users/1`, `/users/2`, etc.