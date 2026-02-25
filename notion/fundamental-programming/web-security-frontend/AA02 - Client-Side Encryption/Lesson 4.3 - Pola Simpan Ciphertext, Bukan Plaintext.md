💡 AA02-4.3: The Golden Rule of Client Storage: Store Ciphertext, Not Plaintext

**Outline:**

- **The Threat (SEEI):** Even if storage like `IndexedDB` is used, the raw data files exist on the user's hard drive. An attacker with physical access or malware could potentially read this data, bypassing the browser's security model.
- **The Defense (PPP):** Implementing the core pattern of client-side encryption: application memory can contain plaintext, but persistent storage must _only_ contain ciphertext.
- **Your Mission (Production):** Time to implement a pair of functions that securely save to and load from `localStorage` using this pattern.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Plaintext at Rest on the Client.** We know storing sensitive data in `localStorage` is bad because of XSS. So what if we use `IndexedDB`? It's more secure, right? While it's true that `IndexedDB` isn't a simple global object, its underlying data is still stored in database files on the user's computer. If an attacker steals a user's laptop and can bypass the operating system login, they can mount the hard drive and directly access the browser's profile files. If your application stored the user's diary entries or medical records as plaintext in `IndexedDB`, the attacker can read them all.
- **How it Works (The Attack Vector):**
    1. A user uses a "secure" note-taking app that stores notes in `IndexedDB` for offline access.
    2. The user's laptop is stolen.
    3. The thief boots the laptop from a USB stick with a different operating system.
    4. They browse the file system to the original user's profile directory (e.g., `.../AppData/Local/Google/Chrome/User Data/Default/databases/...`).
    5. They find the SQLite files that back the `IndexedDB` storage for the note-taking app's origin.
    6. They open these files with a standard SQLite browser and can read all the "secure" notes in plaintext.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **The client's persistent storage is an untrusted, public space. Only store data there after it has been encrypted.** The key required for decryption must only ever exist in the application's memory during an active, authenticated session.
    
- **Secure Code Implementation (The Workflow):** Let's combine our knowledge from previous lessons into a secure save/load workflow.
    
    ```ts
    import { encryptData } from './encryptor'; // Assumes our encrypt function
    import { decryptData } from './decryptor'; // Assumes our decrypt function
    import { useAuthStore } from './authStore'; // Our store with the in-memory key
    
    const STORAGE_KEY = 'secure_note';
    
    /**
     * Encrypts the note content and saves the ciphertext to localStorage.
     */
    function saveNoteSecurely(plaintextNote: string): void {
      const key = useAuthStore.getState().inMemoryKey;
      if (!key) {
        console.error("Cannot save: User is not logged in.");
        return;
      }
    
      // 1. Encrypt the data before saving.
      const ciphertext = encryptData(plaintextNote, key);
    
      // 2. Save ONLY the ciphertext to storage.
      localStorage.setItem(STORAGE_KEY, ciphertext);
      console.log("Note saved securely.");
    }
    
    /**
     * Loads the ciphertext from localStorage and decrypts it.
     * @returns The plaintext note, or null if not found or decryption fails.
     */
    function loadNoteSecurely(): string | null {
      const key = useAuthStore.getState().inMemoryKey;
      if (!key) {
        console.error("Cannot load: User is not logged in.");
        return null;
      }
    
      // 1. Load the ciphertext from storage.
      const ciphertext = localStorage.getItem(STORAGE_KEY);
      if (!ciphertext) {
        return null; // No note saved yet.
      }
    
      try {
        // 2. Decrypt the data after loading.
        const plaintext = decryptData(ciphertext, key);
        return plaintext;
      } catch (error) {
        console.error("Failed to decrypt note:", error);
        return null; // Key is wrong or data is corrupt.
      }
    }
    
    ```
    

### 🧠 Real-World Case Study: "The Coded Diary"

- **The Goal:** Keep a private diary.
- **The System:**
    - **Your Desk:** Your application's active memory (`RAM`).
    - **Your Desk Drawer:** Persistent client-side storage (`localStorage` or `IndexedDB`).
    - **The Diary:** Your user's data.
    - **Your Secret Language:** Your encryption algorithm and key.
- **Vulnerable Approach:** You write in your diary in plain English. When you're done, you put the open diary in your desk drawer. Anyone who can get into your room and open the drawer can read your secrets.
- **Secure Approach:** You invent a secret code that only you know.
    1. When you want to write (`active session`), you take the diary out. You write your entry, but you immediately translate it into your secret code on the page (`encryption`).
    2. When you're finished, you put the diary—now filled with unreadable coded messages (`ciphertext`)—into your desk drawer (`persistent storage`).
    3. If a sibling or parent opens your drawer, all they find is a book of gibberish. To read it, they would need to know your secret code (`the key`), which exists only in your head (`in-memory`).

### 🤔 Reflective Questions

1. This pattern provides excellent security for data at rest. Does it do anything to protect data from a successful XSS attack while the user is logged in and the key is in memory?
2. How does this pattern make features like "full-text search" of all notes much more complicated to implement?
3. If you use this pattern but store the ciphertext in `localStorage`, is it now "safe"? What are the pros and cons compared to storing it in `IndexedDB`?

### 📚 Further Reading

- **OWASP:** [Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- **Bitwarden Blog:** [How End-to-End Encryption Secures Your Data](https://www.google.com/search?q=https://bitwarden.com/blog/how-end-to-end-encryption-secures-your-data/) (Bitwarden is a password manager that uses this pattern).
- **Trail of Bits Blog:** [A Formal Security Analysis of the Standard Notes Protocol](https://www.google.com/search?q=https://blog.trailofbits.com/2019/12/16/a-formal-security-analysis-of-the-standard-notes-protocol/)

### 📝 Mini-Task (Production)

You will implement the secure storage pattern.

1. Start with the `encryptor.ts` and `decryptor.ts` files you built in Module 2.
2. Create a mock in-memory key: `const MOCK_KEY = "a-very-strong-secret-key";`
3. Implement the two functions, `saveNoteSecurely(plaintextNote: string)` and `loadNoteSecurely(): string | null`, as shown in the lesson. For this task, they can use your `MOCK_KEY` instead of a state management store.
4. In a main `index.ts` file, first call `saveNoteSecurely("My secret diary entry.")`.
5. Then, immediately call `loadNoteSecurely()` and log the result to the console, verifying that it returns the original plaintext.
6. Use your browser's dev tools to inspect `localStorage` and confirm that the value stored is unreadable ciphertext.