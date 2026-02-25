# 💡 CM01-1.4: The Finishing Touches: Customizing Look, Feel, and Splash Screen

**Outline:**

- **Beyond the Icon (Presentation):** Exploring properties that control the app's appearance: `display`, `theme_color`, and `background_color`.
- **Polishing the Experience (Practice):** An example manifest showing how to define a custom theme and create a splash screen.
- **Crafting Your App's Shell (Production):** Time to customize your PWA's installed appearance.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond the Icon**

We've given our app an identity. Now, let's give it a personality. The manifest allows us to control how our app looks when it's launched from the home screen, making it feel much more integrated with the operating system.

1. **`display`**: This is a powerful one. It controls how much of the browser UI is shown.
    - `"standalone"`: This is the most common choice for PWAs. It opens the app in its own window, without the browser address bar, back buttons, or other UI. It looks and feels like a native app.
    - `"fullscreen"`: This goes a step further and takes over the entire screen, hiding even the system status bar (time, battery, etc.). It's great for immersive experiences like games or video players.
    - `"minimal-ui"`: A middle ground. It provides its own window but might include a minimal set of browser controls like back and refresh.
    - `"browser"`: The default. It just opens the app in a normal browser tab.
2. **`theme_color`**: This sets the color of the toolbar or title bar of your app's window. It should match your brand's primary color. It's a small touch that makes the app feel cohesive and professionally designed. The browser also uses this to color the address bar when users are just visiting your site in a regular tab.
3. **`background_color`**: This property defines the background color of the **splash screen**. When a user launches your PWA, it can take a moment for the web content to load. The OS will immediately show a simple screen with your app icon and this background color. This provides instant feedback to the user and makes the startup feel faster and smoother than a blank white screen.

### **Part 2: Practice - Polishing the Experience**

Let's add these new properties to our manifest. The combination of `background_color`, `theme_color`, and your `icons` creates an auto-generated splash screen for a polished launch experience.

```json
{
  "name": "WeatherHub - Real-time Forecasts",
  "short_name": "WeatherHub",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#007bff",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ]
}
```

**Note on `purpose: "maskable"`**: This is an advanced hint for one of your icons. It tells the OS that this icon is designed to be safely cropped into different shapes (like circles or squares with rounded corners) without cutting off important parts of the logo. It's a best practice for ensuring your icon looks great on all Android devices.

## 🧠 **Real-World Case Study: "The Flash of White"**

- **Before Customization:** A user taps the PWA icon on their home screen. For a full second, they see a jarring, blank white screen before the app's content finally paints. This momentary flash, while small, feels slow and unrefined. It breaks the illusion of a native app.
- **After Customization:** The developer adds `background_color` and `theme_color`. Now, when the user taps the icon, the OS instantly displays a splash screen with the app's background color and icon. This screen persists until the app is ready to render, providing a seamless transition. The perceived performance of the app skyrockets, even if the actual load time is the same. It feels intentional and polished.

## 🤔 **Reflective Questions**

1. For what type of application would `display: "fullscreen"` be a better choice than `display: "standalone"`?
2. How should you choose your `theme_color` and `background_color` to create the best user experience? Should they be the same? Different?
3. If you set a `theme_color` in your manifest, you can also set one with a `<meta>` tag in your HTML. Which one do you think takes precedence?

## 📚 **Further Reading**

- **web.dev Splash Screens:** [Customizing your PWA's splash screen](https://www.google.com/search?q=https://web.dev/articles/splash-screen)
- **MDN `display` member:** [MDN documentation for the `display` property](https://developer.mozilla.org/en-US/docs/Web/Manifest/display)
- **Maskable Icons Tool:** [An excellent tool to check if your icon is "maskable"](https://maskable.app/)

## 📝 **Mini Task (Production)**

Let's give your app a custom look.

**Task:**

1. Open your `manifest.json` file.
2. Add the `display`, `theme_color`, and `background_color` properties.
3. Set `display` to `"standalone"`.
4. Choose a hex color code for your `theme_color` (e.g., your brand's main color).
5. Choose a hex color code for your `background_color` (often white `#ffffff` or a light grey is a safe bet).
6. Re-run the Lighthouse PWA audit. You should now pass the "Sets a theme color for the address bar" and "Configures a custom splash screen" checks.
7. If you are testing on an Android device, try adding the app to your home screen and launching it to see the effects!