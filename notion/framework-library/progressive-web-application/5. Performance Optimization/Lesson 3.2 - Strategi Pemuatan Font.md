# 💡 AM01.3.2: The Balancing Act: Modern Font Loading Strategies

**Outline:**

- **The Hidden Cost of Typography (Presentation):** Understanding how web fonts can block rendering and cause layout shifts.
- **Taking Control with `font-display` (Practice):** Implementing the single most effective CSS property for font loading.
- **Preloading Critical Fonts (Production):** Giving the browser a hint to fetch your most important font even earlier.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Hidden Cost of Typography**

Custom web fonts are essential for branding and design, but they are also a render-blocking resource and can seriously harm your Core Web Vitals if not handled correctly. When the browser sees you're using a custom font, it must download that font file before it can display the text. This leads to two common problems:

1. **FOIT (Flash of Invisible Text):** The browser hides the text completely while it waits for the font file to download. On a slow connection, your users might see blank headlines for several seconds. This directly harms your LCP.
2. **FOUT (Flash of Unstyled Text):** The browser shows a system fallback font (like Arial) first, and then _swaps_ it with your custom font once it loads. While this is better than invisible text, the swap can cause a jarring shift in layout (CLS) if the fallback font has different dimensions.

Our goal is to find a balance: show text to the user as quickly as possible (avoiding FOIT) while minimizing the jarring layout shift (CLS) from FOUT.

### **Part 2: Practice - Taking Control with `font-display`**

The most powerful tool we have for this is the `font-display` CSS descriptor, which we add to our `@font-face` rule. It tells the browser _how_ to behave while the font is loading.

```css
@font-face {
  font-family: 'MyAwesomeFont';
  src: url('/fonts/my-awesome-font.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  /* This is the magic property! */
  font-display: swap;
}
```

The `swap` value is the workhorse for modern font loading. It tells the browser:

- **Block period (0ms):** Don't wait at all. If the font isn't ready, immediately show the text using the fallback font. (This eliminates FOIT).
- **Swap period (infinite):** As soon as the custom font finishes downloading, swap it with the fallback font.

This gives us the best of both worlds: the content is always readable, and we get our custom typography as soon as it's available. To minimize the CLS from the swap, you can try to match the fallback font's size and line-height to your custom font using tools, but simply using `font-display: swap` is the biggest and most important step.

## **🧠 Real-World Case Study: "The Invisible News Headlines"**

- **The Problem:** A major online newspaper was using a custom brand font for all their headlines. On slower mobile connections, users reported that the top half of the page was just blank for 3-5 seconds, even though the article text (in a system font) was visible below. Their LCP was abysmal.
- **The Diagnosis:** DevTools confirmed that the browser was waiting for the large headline font file to download before rendering any of the `<h1>` or `<h2>` tags, a classic case of FOIT. Their `@font-face` rule had no `font-display` property, so the browser was using its default `auto` behavior (which often acts like `block`).
- **The Fix:** A single line of code was added to their CSS: `font-display: swap;`.
- **The Result:** On the next deployment, the headlines became immediately visible on all connections, rendered in a system font like Times New Roman. A second or two later, the brand font would swap in. The user could start reading immediately. It was a massive improvement in perceived performance and LCP, all from one CSS property.

## 🤔 **Reflective Questions**

1. What are the other possible values for `font-display` (like `block`, `fallback`, `optional`), and in what specific scenarios might you use them?
2. Why is the `woff2` font format the best one to use for modern browsers?
3. If `font-display: swap` can still cause CLS, what is one technique you could use to minimize the size difference between the fallback font and your web font?

## 📚 **Further Reading**

- **Web.dev:** [Controlling font performance with `font-display`](https://www.google.com/search?q=https://web.dev/articles/font-display)
- **CSS-Tricks:** [The `@font-face` rule and `font-display`](https://css-tricks.com/almanac/properties/f/font-display/)

## 📝 **Mini Task (Production)**

Your mission is to eliminate FOIT from your application.

1. Find the `@font-face` declaration for the primary web font used in your application (it might be in your own CSS or hosted on a service like Google Fonts).
2. Add the `font-display: swap;` descriptor to this rule.
3. Use the "Network" tab in DevTools to simulate a "Slow 3G" connection.
4. Reload your page. You should now see your text appear instantly in a fallback font before your custom font loads and swaps in. You've successfully defeated the Flash of Invisible Text!