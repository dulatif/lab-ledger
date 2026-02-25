### **💡 AE01-2.3: Beaming It Up: Sending Your Vitals Data**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Data trapped in the user's browser is useless.
- **The Optimization Technique (Practice):** A hands-on guide to using `navigator.sendBeacon` to reliably send analytics.
- **Your Performance Mission (Production):** Time to send your vitals data to a mock endpoint.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to reliably transmit the performance data we've collected from the user's browser to a central server for storage and analysis. We must do this in a way that is both efficient and doesn't interfere with the user's experience.
- **Identifying the Problem:** We can now collect vitals, but they only exist in the user's browser console. This data is ephemeral and isolated. The bottleneck is the **data transport layer**. How do we get this data from potentially millions of browsers back to our servers without losing data and without slowing down the page, especially when the user is navigating away or closing the tab?

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** A standard `fetch()` call is not ideal for analytics. If the user closes the tab, the browser may cancel the `fetch` request before it completes, leading to data loss. The correct tool for this job is the `navigator.sendBeacon()` Web API.
    
- **Why `sendBeacon`?**
    
    - **Reliable:** The browser queues the request and guarantees it will be sent, even if the page is unloading.
    - **Asynchronous & Non-blocking:** It's fired off without waiting for a response, so it doesn't block the main thread or delay page navigation.
    - **Optimized:** The browser can be smart about when to send the data, for example, waiting for idle network time.
- **Secure Code Implementation:** We'll create a single function to handle sending the data and pass it to our `web-vitals` callbacks.
    
    ```
    import { onLCP, onINP, onCLS } from 'web-vitals';
    
    function sendToAnalytics(metric) {
      // Create a payload. You can add more context here.
      const body = JSON.stringify({
        name: metric.name,
        value: metric.value,
        id: metric.id,
      });
    
      // Use `navigator.sendBeacon()` if available, with a fallback to `fetch`.
      if (navigator.sendBeacon) {
        navigator.sendBeacon('/api/vitals', body);
      } else {
        // `keepalive: true` is essential for the fetch fallback.
        fetch('/api/vitals', { body, method: 'POST', keepalive: true });
      }
    }
    
    onLCP(sendToAnalytics);
    onINP(sendToAnalytics);
    onCLS(sendToAnalytics);
    
    ```
    
    This code defines a robust `sendToAnalytics` function that takes a metric, stringifies it, and sends it to our `/api/vitals` endpoint using the best available method.
    

### 🧠 **Real-World Case Study: "The Lost Conversions"**

- **The Scenario:** An online retailer wants to track a custom metric: the time it takes from a user clicking "Add to Cart" to the confirmation message appearing.
- **The Problem:** They implemented this tracking with a standard `fetch` call. Their data showed very few users completing the action, which seemed wrong. They realized that after the confirmation appeared, many users would immediately navigate to the checkout page. This navigation would cancel their `fetch` request, so the "success" event was never recorded.
- **The Solution:** They replaced their `fetch` call with `navigator.sendBeacon`. Immediately, their data collection became far more accurate. They discovered that their "Add to Cart" process was actually working well; they were just failing to measure it properly. Using the right tool for data beaconing was critical.

### 🤔 **Reflective Questions**

1. What is the `keepalive: true` option for the `fetch` API? How does it attempt to replicate the behavior of `sendBeacon`?
2. `sendBeacon` sends a `POST` request. Why not a `GET` request? What are the limitations on the amount of data you can send?
3. Imagine your `/api/vitals` endpoint is slow or goes down. How does `sendBeacon`'s "fire-and-forget" nature affect your ability to know if the data was successfully received?

### 📚 **Further Reading**

- **MDN:** [`navigator.sendBeacon()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon)
- **MDN:** [`fetch()` API `keepalive` option](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/API/fetch%23keepalive)
- **W3C Beacon Specification:** [Beacon Spec](https://www.w3.org/TR/beacon/) (For a deep dive into the standard).

### 📝 **Mini Task (Production)**

- **Your Mission:** Send your collected vitals data to a live (but temporary) API endpoint.
    1. Go to a request bin service like [Beeceptor](https://beeceptor.com/) and create a free endpoint. This will give you a URL that can receive and display any POST requests sent to it.
    2. Take your code from the previous lesson that uses `web-vitals`.
    3. Implement the `sendToAnalytics` function from the "Practice" section above.
    4. **Important:** Replace the `'/api/vitals'` URL with the unique endpoint URL you got from Beeceptor.
    5. Load your application. As the vitals are measured, you should see the POST requests appear live on your Beeceptor dashboard, containing your JSON payload.