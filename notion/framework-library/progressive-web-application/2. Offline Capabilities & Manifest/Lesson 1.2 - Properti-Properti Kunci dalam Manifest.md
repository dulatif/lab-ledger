# 💡 CM01-1.2: Your App's ID Card: Key Manifest Properties

**Outline:**

- **Defining the Essentials (Presentation):** Understanding the core properties that give your app its identity (`name`, `short_name`, `icons`, `start_url`).
- **Crafting the Manifest (Practice):** A hands-on example of a well-formed manifest with multiple icons.
- **Building Your Identity (Production):** Time to populate your manifest with your app's information.

## 📘 **Full Lesson Content**

**Part 1: Presentation - Defining the Essentials**

Now that you have your `manifest.json` file, let's turn it into a proper ID card for your application. There are four properties that form the absolute foundation of an installable PWA. Getting these right is critical for a good user experience.

1. **`name` & `short_name`**:
    - `name`: This is the full, official name of your application. It's what will be shown in the installation prompt and often in the app store listing if you go that route. Example: `"Super Productive Task Manager"`.
    - `short_name`: This is the name displayed on the user's home screen where space is limited. Keep it short and recognizable. Example: `"Tasks"`. If `short_name` isn't provided, the browser will try to use `name`, which often gets truncated awkwardly.
2. **`icons`**:
    - This is an array of image objects. It's your app's logo. You must provide icons in various sizes so the operating system can pick the best one for the context (e.g., home screen, app switcher, notification panel). The browser needs at least one icon, typically 192x192 and 512x512 pixels, to show the "Add to Home Screen" prompt.
3. **`start_url`**:
    - This is the entry point of your application when a user launches it from their home screen. It's the URL that gets loaded. Usually, you'll want this to be the root of your site (`/`) or a specific entry point like `/app`. It ensures users always start in the right place.

**Part 2: Practice - Crafting the Manifest**

Let's expand on our basic manifest from the last lesson. Notice how the `icons` property is an array of objects, each specifying the source (`src`), `sizes`, and `type`.

```json
{
  "name": "WeatherHub - Real-time Forecasts",
  "short_name": "WeatherHub",
  "start_url": "/",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

**Important:** The paths in `src` are relative to the location of the manifest file. Since `manifest.json` is in the `public` folder, a path like `/icons/icon-192x192.png` means the browser will look for the icon in `public/icons/icon-192x192.png`.

## 🧠 **Real-World Case Study: "Iconography Matters"**

- **Bad Experience:** A PWA provides only a single, low-resolution 48x48 icon. When a user adds it to their modern high-resolution phone screen, the OS stretches the icon. It looks blurry and unprofessional, sitting next to crisp, native app icons. This small detail immediately makes the app feel "lesser" than its native counterparts.
- **Good Experience:** A PWA provides a range of icons, including a high-density 512x512 version. On any device, the OS picks the perfect size. The icon is sharp and clear on the home screen, in search results, and in the app switcher. The app feels integrated and polished, building user trust.

## 🤔 **Reflective Questions**

1. What could happen if you set your `start_url` to a page that requires a login, like `/dashboard`, but the user isn't logged in yet? How might you handle that?
2. Why is it better to provide multiple icon sizes instead of just one very large one and letting the OS scale it down?
3. If you omit `short_name`, what are the potential UX downsides?

## 📚 **Further Reading**

- **MDN `icons` member:** [MDN documentation for the `icons` property](https://developer.mozilla.org/en-US/docs/Web/Manifest/icons)
- **Web.dev `name` member:** [web.dev article explaining `name` and `short_name`](https://www.google.com/search?q=https://web.dev/articles/add-manifest%23name)

## 📝 **Mini Task (Production)**

Let's give your app its identity.

**Task:**

1. Open the `manifest.json` file you created in the last lesson.
2. Add the `name`, `short_name`, `icons`, and `start_url` properties.
3. For the icons, you don't need perfect graphics right now. You can use a placeholder image generator (like [placehold.co](https://placehold.co/)) to create 192x192 and 512x512 PNG files.
4. Create an `icons` folder inside your `public` directory and place your placeholder images there.
5. Make sure the `src` paths in your manifest correctly point to these new icons.