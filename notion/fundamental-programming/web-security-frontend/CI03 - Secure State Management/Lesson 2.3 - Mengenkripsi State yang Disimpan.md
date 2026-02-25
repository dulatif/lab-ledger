💡 CI03.2.3: The Secret Safe: Encrypting Persisted State

**Outline:**

- **The Threat (SEEI):** Having accepted that `localStorage` is insecure, we face the problem of needing to persist sensitive data (like a session token) for a good user experience. Storing it in plain text is not an option.
- **The Defense (PPP):** The principle of "Encrypt at Rest." Using a `redux-persist` "transform" to intercept a slice of your state, encrypt it before it's saved to `localStorage`, and decrypt it when it's rehydrated.
- **Your Mission (Production):** Implement `redux-persist-transform-encrypt` to secure a sensitive `auth` slice.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The vulnerability is **plain text persistence of sensitive data**. We know `localStorage` is a public notebook. We know `redux-persist` is a great tool for UX. When you combine them naively, you get a system that automatically writes your application's secrets into that public notebook for anyone to read.

**How it Works (The Attack Vector):**

With a standard `redux-persist` setup, the data in `localStorage` is a direct, human-readable JSON representation of your state.

**`localStorage` key:** `persist:root**localStorage` value:**

```js
{
  "auth": "{\\"user\\":{\\"id\\":123,\\"name\\":\\"Jane Doe\\"},\\"token\\":\\"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjMifQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c\\"}",
  "cart": "{\\"items\\":[]}",
  "_persist": "{\\"version\\":-1,\\"rehydrated\\":true}"
}

```

An attacker with XSS or physical access doesn't need to do any work. They can just copy the `token` string and use it to impersonate the user. We have provided them the keys to the kingdom.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Sensitive data must be encrypted before it touches the disk."** We can't make `localStorage` itself secure, but we can control what we put into it. A "transform" in `redux-persist` is a plugin that sits between your Redux store and the storage engine. It gets the last look at the data before it's saved and the first look when it's loaded. This is the perfect place to apply encryption.

**Secure Code Implementation (Using `redux-persist-transform-encrypt`):**

First, install the library: `npm install redux-persist-transform-encrypt`

Next, we update our `redux-persist` configuration.

```ts
// SECURE CODE
import { configureStore } from '@reduxjs/toolkit';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';
import { encryptTransform } from 'redux-persist-transform-encrypt'; // Import the transform
import rootReducer from './reducers';

// 1. Define the transform
const encryptor = encryptTransform({
  secretKey: 'my-super-secret-key', // We will discuss this key in the next lesson!
  // You can also add an error handler, e.g. onError: (error) => { ... }
});

// 2. Update the persist config
const persistConfig = {
  key: 'root',
  storage,
  // Add the transforms array
  transforms: [encryptor],
  // We only want to encrypt the 'auth' slice, so we whitelist it for persistence
  whitelist: ['auth'],
};

const persistedReducer = persistReducer(persistConfig, rootReducer);
// ... the rest of the store setup is the same

```

Now, when the state is saved, `redux-persist` will take the `auth` slice, pass it to the `encryptor`, which will turn it into a string of gibberish, and only then will it be saved to `localStorage`. When the app loads, the reverse happens.

**`localStorage` value (After Encryption):**

```js
{
  "auth": "{\\"_isEncrypted\\":true,\\"ct\\":\\"...\\",\\"iv\\":\\"...\\",\\"s\\":\\"...\\"}", // Encrypted gibberish!
  "_persist": "{\\"version\\":-1,\\"rehydrated\\":true}"
}

```

An attacker who steals this data can't use it without the `secretKey`. We have successfully protected the data "at rest."

### 🤔 Reflective Questions

1. In our example, we are only encrypting the `auth` slice. Why might you choose _not_ to encrypt other slices like `ui` or `cart`? What is the performance trade-off?
2. The `encryptTransform` requires a `secretKey`. Where is the logical flaw in hardcoding this key directly in your client-side JavaScript bundle as we did in the example?

### 📚 Further Reading

- **redux-persist-transform-encrypt:** [GitHub Repository](https://www.google.com/search?q=https://github.com/maxence-charriere/redux-persist-transform-encrypt) - The official documentation for the library.

### 📝 Mini Task (Production)

You are using `redux-persist-transform-encrypt`. Your state has an `auth` slice (which should be encrypted) and a `userSettings` slice (which also contains sensitive PII and should be encrypted). It also has a non-sensitive `theme` slice that you also want to persist.

Your task: Write the `persistConfig` object that correctly persists all three slices but applies the encryption transform only to the two sensitive slices. (Hint: `whitelist` applies to the transform, not just the root config).