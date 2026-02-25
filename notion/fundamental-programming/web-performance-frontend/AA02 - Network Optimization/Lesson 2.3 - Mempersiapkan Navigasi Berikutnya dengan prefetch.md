### **💡 AA02-2.3: Preparing for the Next Navigation with `prefetch`**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The user's wait time _after_ clicking a link.
- **The Optimization Technique (Practice):** A hands-on guide to implementing `prefetch` for future routes.
- **Your Performance Mission (Production):** Time to make the checkout process feel faster.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to make navigation between pages in our application feel instantaneous. We are optimizing the perceived performance of user-initiated actions, specifically the time between a user clicking a link and the next page becoming interactive.
- **Identifying the Problem:** When a user clicks a link to a new page (e.g., from `/cart` to `/checkout`), the browser has to start downloading all the necessary assets for `/checkout` from scratch. This includes the HTML, CSS, and especially the JavaScript bundles required for that page. This download time is the bottleneck. The user clicks, and then they wait.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We use `<link rel="prefetch">` to give the browser a low-priority hint to download resources that the user will _probably_ need in the near future. The browser uses its idle time to fetch these resources and store them in its cache. When the user eventually navigates to the page, the assets are already available locally, and the page load is dramatically faster.
    
- **Secure Code Implementation:**
    
    Imagine a user is on a product listing page. It's highly likely they will click on a product to see its details.
    
    ```html
    <!-- On the /products page -->
    <head>
      <!-- ... other head tags ... -->
    
      <!-- Let's prefetch the JS bundle for the product details page -->
      <!-- This is a LOW priority hint. Browser will do it when it's free. -->
      <link rel="prefetch" href="/_next/static/chunks/pages/products/[id].js" as="script">
    </head>
    <body>
      <a href="/products/cool-sneakers">View Cool Sneakers</a>
      <!-- ... -->
    </body>
    
    ```
    
    When the user is browsing the `/products` page, the browser will quietly download the product detail page's JS in the background. When they click the link, the JS is read from the cache instead of the network.
    

### 🧠 **Real-World Case Study: "The Instant E-Commerce Journey"**

- **The Scenario:** An e-commerce site wants to optimize the path from the shopping cart to the checkout page.
- **The Problem:** The checkout page is a complex React component with its own JS bundle, including logic for address validation and payment processing. When users click "Proceed to Checkout," there's a noticeable 1-2 second delay while `checkout.js` downloads before the page can render. This delay can lead to user frustration and cart abandonment.
- **The Solution:** On the cart page (`/cart`), the team adds a simple hint: `<link rel="prefetch" href="/checkout.js" as="script">`. While the user is reviewing their cart, the browser downloads `checkout.js` in the background. When they finally click the checkout button, the browser already has the script. The transition feels instant, creating a smooth and professional user experience that encourages completion of the purchase.

### 🤔 **Reflective Questions**

1. What's the difference between browser caching (e.g., via `Cache-Control` headers) and prefetching?
2. If `prefetch` is so great, why not just prefetch every single asset for your entire website on the homepage? What would be the downside? (Hint: think about data usage).
3. How does the browser decide when it's "idle"? What might cause a `prefetch` request to be delayed or cancelled?

### 📚 **Further Reading**

- **MDN:** [`<link rel="prefetch">`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/prefetch)
- **web.dev:** [Prefetch resources to speed up future navigations](https://web.dev/link-prefetch/)
- **CSS-Tricks:** [Prefetching, Preloading, Pre-browsing](https://www.google.com/search?q=https://css-tricks.com/prefetching-preloading-pre-browsing/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are working on a web application that has a multi-step user registration flow. The user starts on `/signup/step1` and will then proceed to `/signup/step2`.
    1. In the HTML for the `step1` page, add a `<link rel="prefetch">` tag.
    2. The tag should target the JavaScript bundle for the second step, which is located at `/js/signup-step2.bundle.js`.
    3. Use your browser's DevTools Network tab. Load the `step1` page. You should see the request for `signup-step2.bundle.js` appear in the list with a "Lowest" priority.
    4. Verify that when you navigate to `step2`, the bundle is served "from disk cache" or "from memory cache".