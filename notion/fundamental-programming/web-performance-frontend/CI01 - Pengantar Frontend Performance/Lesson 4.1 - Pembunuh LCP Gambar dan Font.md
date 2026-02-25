💡 CI01-4.1: LCP Killers - Images and Fonts

Outline:

- **The Metric & The Bottleneck (Presentation):** Identifying the two most common culprits for a slow LCP.
- **The Optimization Technique (Practice):** A checklist for diagnosing and fixing slow images and fonts.
- **Your Performance Mission (Production):** Auditing a slow page and identifying its LCP bottleneck.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Today, we hunt down the most frequent killers of the **Largest Contentful Paint (LCP)** metric. A good LCP score is critical for making a site _feel_ fast.
- **Identifying the Problem:** More often than not, a slow LCP is caused by one of two things:
    1. **Unoptimized Images:** The LCP element is a large hero image that is too big in file size, has the wrong dimensions, or is in an outdated format.
    2. **Slow-Loading Web Fonts:** The LCP element is a block of text that can't be rendered until a large, custom font file has been downloaded and parsed from a third-party server (e.g., Google Fonts). The browser is stuck waiting.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The technique is a process of elimination. First, identify your LCP element. If it's an image or text, follow a specific diagnostic checklist.
    
- **Secure Code Implementation (The Diagnostic Checklist):**
    
    **If your LCP element is an IMAGE:**
    
    1. **Is it compressed?** An uncompressed 2MB PNG hero image is a performance crime. Use a tool like [Squoosh](https://squoosh.app/) to compress it.
    2. **Is it in a modern format?** Convert JPEGs and PNGs to **WebP** or **AVIF**. They offer better compression and quality.
    3. **Is it correctly sized?** Don't serve a 4000px wide image for a container that is only 800px wide. Resize the image to match its display size.
    4. **Is it lazy-loaded incorrectly?** The LCP image should **NEVER** be lazy-loaded. It needs to load as quickly as possible. Ensure your `<img>` tag does _not_ have `loading="lazy"`.
    5. **Is it high-priority?** Help the browser discover it sooner. Add `fetchpriority="high"` to your LCP `<img>` tag.
    
    **If your LCP element is TEXT using a web font:**
    
    6. **Are you self-hosting?** Relying on a third-party server (like `fonts.googleapis.com`) adds an extra DNS lookup and connection, which slows things down. Self-hosting the font files from your own domain is often faster.
    7. **Are you preloading the font?** Tell the browser to download the critical font file early by adding `<link rel="preload">` in your HTML `<head>`.
    8. **Are you using `font-display: swap`?** This CSS property tells the browser to show the text immediately with a fallback system font, and then "swap" it for the web font once it loads. This dramatically improves FCP and perceived performance, even if it doesn't directly speed up the font download.

### 🧠 **Real-World Case Study: "The `fetchpriority` Fix"**

- **The Scenario:** An e-commerce site's product page features a large, high-quality image of the product as its LCP element. Despite optimizing the image size and format, the LCP score was still "Needs Improvement."
- **The Analysis:** Using the DevTools Network tab, they saw that the browser was discovering and downloading other resources—like CSS and non-critical JavaScript—_before_ it started downloading the main product image.
- **The Bottleneck:** The browser's default priority heuristics didn't consider this specific image as the most important resource on the page.
- **The Takeaway:** The developers added `fetchpriority="high"` to the `<img>` tag. This one-line change acted as a direct signal to the browser: "This is the most important resource. Download it now." Re-running the tests showed the image download started much earlier, improving the LCP by over 600ms and moving it into the "Good" category.

### 🤔 **Reflective Questions**

1. What are the trade-offs between self-hosting fonts and using a service like Google Fonts? (Hint: think about caching and maintenance).
2. Why should the LCP image never be lazy-loaded? What is the purpose of lazy loading?
3. The `<picture>` element allows you to serve different image formats (like AVIF, WebP, and JPG) to different browsers. How does this help with performance optimization?

### 📚 **Further Reading**

- **Image Optimization:** [A deep dive into optimizing images for the web](https://www.google.com/search?q=https://web.dev/articles/optimize-images)
- **Font Optimization:** [Best practices for optimizing web fonts](https://www.google.com/search?q=https://web.dev/articles/optimize-web-fonts)
- **Preload Critical Assets:** [Learn how to use rel="preload" to speed up LCP](https://web.dev/articles/preload-critical-assets)

### 📝 **Mini Task (Production)**

- **Your Mission:** Go to a website with a prominent hero image (e.g., a movie promotion site, a car manufacturer's homepage).
- **Your Deliverable:**
    1. Use DevTools to identify the LCP element.
    2. If it's an image, inspect it. What format is it (JPG, PNG, WebP)? Right-click and "Open image in new tab" to see its full dimensions. Is it being served at a much larger size than it's displayed?
    3. Write two sentences summarizing your findings. "The LCP element on [website] is an image. It could be optimized by [e.g., converting from JPG to WebP, resizing it from 3000px to 1200px wide]."