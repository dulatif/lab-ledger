# 💡 AM01.3.1: More Than a Pretty Face: Mastering Image Optimization

**Outline:**

- **The Silent Performance Killer (Presentation):** Understanding why images are often the heaviest assets and the pillars of optimization.
- **Hands-On Compression (Practice):** Using modern tools and techniques to shrink image sizes without sacrificing quality.
- **Optimizing Your Hero (Production):** Applying a full suite of image optimizations to a critical page element.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Silent Performance Killer**

Team, we've spent a lot of time on JavaScript, but often the single heaviest asset on a page is an image. A beautiful, high-resolution hero image can easily weigh more than all your JavaScript combined, and it can single-handedly destroy your LCP score.

Mastering image optimization isn't just a "nice-to-have"; it's a fundamental requirement for a performant PWA. The strategy rests on four pillars:

1. **Format:** Stop using JPEG and PNG for everything. Modern formats like **WebP** and **AVIF** offer vastly superior compression and features. They can reduce file size by 30-50% with little to no visible quality loss. We should serve these formats to modern browsers while providing a fallback.
2. **Compression:** Find the right balance between file size and visual quality. A "lossy" compression level of 80% is often visually indistinguishable from 100% but can cut the file size in half.
3. **Sizing:** Don't serve a 4000px wide desktop image to a 360px wide mobile screen. This is incredibly wasteful. We must use responsive image techniques (`srcset` and `sizes` attributes) to let the browser download an appropriately sized version for its viewport.
4. **Loading:** Images that are "below the fold" (not visible in the initial viewport) don't need to be loaded immediately. We can use **lazy loading** to defer their download until the user scrolls them into view.

Applying these four techniques systematically will have a massive impact on your page load times and your Core Web Vitals.

### **Part 2: Practice - Hands-On Compression**

The best way to understand this is to do it. Let's use [Squoosh.app](https://squoosh.app/), a fantastic web-based tool from Google.

1. Find a large, unoptimized image (a high-quality photo works best).
2. Drag it into Squoosh.
3. On the right-hand side, under "Compress", change the format from "MozJPEG" to **"WebP"**.
4. Look at the bottom right corner. Notice the incredible percentage decrease in file size, often over 70%! Use the slider to compare the before and after—the quality is likely almost identical.
5. Now, let's implement responsive images in HTML. The `<picture>` element is perfect for this:
    ```html
    <picture>
      <!-- Serve the modern WebP format to browsers that support it -->
      <source srcset="hero-image.webp" type="image/webp">
      <!-- Fallback to the old JPG format for older browsers -->
      <source srcset="hero-image.jpg" type="image/jpeg">
      <!-- The final img tag is for content and is the ultimate fallback -->
      <img src="hero-image.jpg" alt="A descriptive alt text.">
    </picture>
    ```
6. For lazy loading, the modern approach is incredibly simple. Just add one attribute:
    ```html
    <img src="article-image.jpg" alt="..." loading="lazy" width="600" height="400">
    ```
    The browser will now automatically defer loading this image until it's close to the viewport. (Note: Providing `width` and `height` is crucial to prevent layout shifts!)

## **🧠 Real-World Case Study: "The Slow-Loading Product Page"**

- **The Problem:** An e-commerce site's product pages had a terrible LCP score (> 6 seconds). Users on mobile would see a blank space for a long time where the main product photo was supposed to be.
- **The Diagnosis:** Lighthouse and WebPageTest revealed the cause: they were serving a single, 1.5MB PNG file for the product image to all devices. It was render-blocking and took ages to download on a mobile connection.
- **The Fix:** The team implemented a three-part solution.
    1. They created multiple sizes of each image (small, medium, large).
    2. They converted all images to the WebP format.
    3. They used the `srcset` and `sizes` attributes on the `<img>` tag to serve the correctly sized WebP image based on the user's viewport.
- **The Result:** The average image payload dropped from 1.5MB to just 150KB on mobile. The LCP score fell to 2.1 seconds, moving into the "good" range. User engagement and conversions on mobile saw a measurable increase.

## 🤔 **Reflective Questions**

1. What is the main difference between the WebP format and the older JPEG format?
2. What problem does the `srcset` attribute solve?
3. Why is it critical to include `width` and `height` attributes on an image that is being lazy-loaded? What Core Web Vital does this help prevent?

## 📚 **Further Reading**

- **Web.dev:** [Serve modern image formats](https://www.google.com/search?q=https://web.dev/articles/serve-images-avif-webp)
- **Web.dev:** [Lazy-loading images](https://web.dev/articles/lazy-loading-images)
- **MDN:** [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

## 📝 **Mini Task (Production)**

Take the main hero image or logo from your PWA and give it a full optimization treatment.

1. Run the image through Squoosh.app. Convert it to WebP and apply a reasonable compression level (e.g., quality of 75).
2. In your HTML/JSX, replace the simple `<img>` tag with a `<picture>` element.
3. Provide the new WebP image as the first `<source>` and the original image (JPG/PNG) as a fallback.
4. For another image that is far down your homepage, add the `loading="lazy"` attribute to it. Verify in the Network tab that it only loads when you scroll down.