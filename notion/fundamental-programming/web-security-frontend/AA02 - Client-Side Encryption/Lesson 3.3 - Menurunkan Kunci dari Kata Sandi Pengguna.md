💡 AA02-3.3: The "Just-in-Time" Key: Deriving from a User's Password

**Outline:**

- **The Threat (SEEI):** The core dilemma: we need a key to operate on the client, but we've established there is no safe place to _store_ it on the client.
- **The Defense (PPP):** Reinforcing the KDF (Key Derivation Function) pattern from Lesson 2.4 as the primary solution. The key is generated on-demand from something the user _knows_ (their password) and exists only in application memory.
- **Your Mission (Production):** Time to build a component that manages the in-memory key state, deriving it on login and clearing it on logout.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Dilemma):** We have a paradox. To encrypt or decrypt a user's notes in the browser, our JavaScript code needs access to a secret key. However, we've proven that storing this key in `localStorage` or `sessionStorage` is insecure due to XSS. Storing it in a cookie makes it accessible to the server, which defeats the purpose of client-side encryption. We can't store it "at rest" anywhere on the client. So where does the key come from?
- **How it Works (The Bad Pattern):** A developer, realizing they can't store the key, might be tempted to ask the user for their "encryption key" in a separate prompt every single time they need to perform an action. This would be incredibly annoying for the user and would likely result in them choosing a very simple, insecure key or writing it down on a sticky note, defeating the purpose. The user experience must be viable for the security model to work.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **The user's password is not the key; it is the _source_ of the key.** We solve the storage problem by not storing the key at all. Instead, we regenerate it whenever we need it from two pieces of information: something the user _knows_ (their password) and something we can safely store (their unique salt).
    
- **Secure Code Implementation (React State Management):** This pattern fits perfectly with modern state management. The derived key becomes a piece of application state that is carefully managed.
    
    ```ts
    // store/authStore.ts (using a library like Zustand for state management)
    
    import create from 'zustand';
    import { deriveKey } from '../utils/key-deriver'; // Our PBKDF2 function from Lesson 2.4
    
    interface AuthState {
      // We store the salt, which is safe.
      salt: string | null;
      // The key exists ONLY in memory. It's null when logged out.
      inMemoryKey: string | null;
      login: (password: string, salt: string) => void;
      logout: () => void;
    }
    
    export const useAuthStore = create<AuthState>((set) => ({
      salt: null,
      inMemoryKey: null,
    
      // On login, the user provides their password.
      login: (password, salt) => {
        // We derive the key and set it in our state.
        const key = deriveKey(password, salt);
        set({ inMemoryKey: key, salt: salt });
        console.log("Key derived and stored in memory.");
      },
    
      // On logout, we MUST clear the key.
      logout: () => {
        set({ inMemoryKey: null, salt: null });
        console.log("Key cleared from memory.");
      },
    }));
    
    // --- USAGE IN A COMPONENT ---
    function encryptNote(noteText: string) {
      const key = useAuthStore.getState().inMemoryKey;
      if (!key) {
        throw new Error("User is not logged in. Cannot encrypt.");
      }
      // ...proceed with encryption using the in-memory key
    }
    
    ```
    
    With this pattern, the powerful, 256-bit derived key exists only as a JavaScript variable. When the user logs out or closes the tab, that state is wiped, and the key vanishes.
    

### 🧠 Real-World Case Study: "The Recipe in Your Head"

- **The Goal:** Bake your grandmother's secret recipe chocolate cake.
- **The System:**
    - **Password:** Your memory of the ingredients (`"flour, sugar, eggs, secret ingredient X"`).
    - **Salt:** The specific oven in your kitchen. Baking in a different oven might require slight temperature adjustments.
    - **Derived Key:** The actual, delicious, perfectly baked cake.
    - **Ciphertext:** A photo of the cake.
- **Vulnerable Approach (Storing the key):** You write down the full recipe on an index card and leave it on the kitchen counter (`localStorage`). Anyone who walks into your kitchen can steal it.
- **Secure Approach (Deriving the key):** You don't write the recipe down. You have memorized it.
    1. When you want to bake (`login`), you recall the ingredients from your memory (`enter password`).
    2. You combine them using your knowledge of your specific oven (`use the salt`).
    3. You perform the act of baking (`run KDF`) and produce the cake (`the derived key`).
    4. You use the cake to perform a function (like eating it or taking a picture).
    5. When you're done, you wash the dishes and clean the kitchen (`logout`). The cake is gone, and the recipe is safely back in your head. The key (the cake) only exists "in memory" when you are actively baking.

### 🤔 Reflective Questions

1. What is the main user experience trade-off with this security model? What must the user do every time they start a new session (i.e., open the app on a new day)?
2. How does this pattern protect a user's data if a thief steals their laptop? (Assume the laptop is turned off).
3. In our `useAuthStore` example, the key is a global state. Is there any risk of other, potentially malicious, third-party JavaScript libraries on the same page accessing this state?

### 📚 Further Reading

- **1Password Blog:** [How 1Password Protects Your Data](https://blog.1password.com/how-1password-protects-your-data/) (A real-world example of this architecture).
- **Standard Notes Blog:** [The technicals of building a zero-knowledge web application](https://www.google.com/search?q=https://blog.standardnotes.com/393/the-technicals-of-building-a-zero-knowledge-web-application)
- **Zustand Docs:** [The state management library used in the example](https://github.com/pmndrs/zustand)

### 📝 Mini-Task (Production)

You are to implement the core logic of the `authStore`.

1. Using Zustand or a simple React `useState` in a context provider, create a store or component that holds one piece of state: `inMemoryKey: string | null`.
2. Create a `login` function that accepts a `password` and a `salt`. This function should call a (mock) `deriveKey` function and set the result in the `inMemoryKey` state.
3. Create a `logout` function that sets `inMemoryKey` back to `null`.
4. Create a simple UI with a button to "Login" (which calls your login function with mock data) and a button to "Logout".
5. Display the current value of `inMemoryKey` on the screen to prove that it is being set on login and correctly cleared on logout.