💡 AA02-2.4: The Keymaker: Salting and Key Derivation Functions (KDFs)

**Outline:**

- **The Threat (SEEI):** Using a user's weak, memorable password directly as an encryption key, making it vulnerable to dictionary and rainbow table attacks.
- **The Defense (PPP):** Using a Key Derivation Function (KDF) like PBKDF2, combined with a unique salt, to "stretch" a low-entropy password into a high-entropy, cryptographically strong key.
- **Your Mission (Production):** Time to refactor your encryption logic to derive a key from a password instead of using the password directly.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Weak Keys.** The security of AES-256 is absolute, _assuming_ you have a truly random 256-bit key. A user's password, like `password123` or `Fluffy`, is _not_ random. It has low "entropy" (a measure of randomness). If you use a weak password directly as an encryption key, an attacker doesn't need to break AES. They just need to guess the password. They can do this efficiently with a "dictionary attack," where they try millions of common passwords, or a "rainbow table," which uses pre-computed hashes of common passwords.
- **How it Works (The Attack Vector):** An attacker gets ahold of a user's encrypted data blob. They know the application uses the user's password directly as the AES key.
    1. The attacker takes their list of the 10 million most common passwords.
    2. For each password in the list, they try to decrypt the data blob.
    3. When a decryption attempt produces readable data instead of garbage, they have found the user's password.
    4. This process might only take a few seconds on modern hardware. The strength of AES was completely bypassed by attacking the weak key.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Never use a password directly as a key. Always derive a key from a password.** We use a Key Derivation Function (KDF). A KDF is a slow, deliberate algorithm designed to take a low-entropy input (like a password) and produce a high-entropy output (a cryptographic key).
    
    - **Salt:** A unique, random string that is combined with the password before being fed into the KDF. The salt ensures that even if two users have the same password, the derived keys will be completely different. The salt is not a secret; it's stored and retrieved along with the ciphertext.
    - **Iterations:** The KDF is run many times (e.g., 100,000+ iterations). This makes the process computationally expensive and slow, which is bad for an attacker trying to guess millions of passwords, but acceptable for a user who only does it once per session.
    - **KDF Algorithm:** We'll use **PBKDF2** (Password-Based Key Derivation Function 2), which is readily available in CryptoJS.
- **Secure Code Implementation:** Let's see the improved workflow.
    
    ```ts
    import { AES, enc, lib, PBKDF2 } from 'crypto-js';
    
    // In a real app, you would generate and store this salt per user/per item.
    // For this example, we'll use a static one.
    const salt = lib.WordArray.random(128 / 8).toString(enc.Hex); // Generate a random 16-byte salt
    
    /**
     * Derives a cryptographic key from a password and salt using PBKDF2.
     */
    function deriveKey(password: string, salt: string): string {
      const iterations = 100000; // A good starting point
      const keySize = 256 / 32; // 256-bit key, size in 32-bit words
    
      const key = PBKDF2(password, salt, {
        keySize: keySize,
        iterations: iterations
      });
    
      return key.toString(enc.Hex);
    }
    
    // --- USAGE ---
    const myNote = "This is a much more securely encrypted note.";
    const userPassword = "password123"; // A weak password
    
    // 1. Derive the strong key from the weak password.
    const strongKey = deriveKey(userPassword, salt);
    console.log("Derived Key:", strongKey); // This looks random and is 256 bits long!
    
    // 2. Use the DERIVED key for encryption.
    const ciphertext = AES.encrypt(myNote, strongKey).toString();
    console.log("Ciphertext:", ciphertext);
    
    // To decrypt, you must repeat the EXACT same key derivation process.
    const keyForDecryption = deriveKey(userPassword, salt);
    const decryptedBytes = AES.decrypt(ciphertext, keyForDecryption);
    const decryptedText = decryptedBytes.toString(enc.Utf8);
    console.log("Decrypted Text:", decryptedText);
    
    ```
    

### 🧠 Real-World Case Study: "The Blacksmith's Apprentice"

- **The Goal:** Create an unbreakable sword (a strong key).
- **Vulnerable Approach (Weak Key):** You find a simple iron rod (`a password`) and try to use it as a sword. It's weak, brittle, and will shatter in the first fight (`dictionary attack`).
- **Secure Approach (KDF):** You are a blacksmith's apprentice.
    1. You take the iron rod (`password`).
    2. You add special minerals and carbon (`the salt`).
    3. You heat it in the forge, hammer it, fold it, and repeat this process thousands of times (`iterations`). This is a slow, laborious process (`PBKDF2`).
    4. The final result is a Damascus steel sword (`the derived key`). It is incredibly strong, flexible, and bears no resemblance to the original iron rod. You have "stretched" a weak material into a powerful weapon.

### 🤔 Reflective Questions

1. The salt is not a secret and is stored alongside the encrypted data. How does it still improve security?
2. What is the trade-off involved in choosing the number of iterations for PBKDF2? What would be the consequence of setting it too low? Or too high?
3. Modern alternatives to PBKDF2 exist, such as `scrypt` and `Argon2`. What advantages do they offer?

### 📚 Further Reading

- **OWASP:** [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) (Explains KDFs in detail).
- **CryptoJS on GitHub:** [PBKDF2 Usage](https://www.google.com/search?q=https://github.com/brix/crypto-js%23pbkdf2)
- **Blog Post:** [What is key stretching and why do we need it?](https://www.google.com/search?q=https://www.entersekt.com/what-is-key-stretching-and-why-do-we-need-it/)

### 📝 Mini-Task (Production)

You are tasked with creating a secure key generation utility.

1. Create a file `key-deriver.ts`.
2. Inside this file, write the `deriveKey(password: string, salt: string): string` function as shown in the lesson. Use at least 50,000 iterations. Export this function.
3. Create a `salt.ts` file that generates a single random 16-byte salt using `CryptoJS.lib.WordArray.random(128 / 8).toString()` and exports it as a constant.
4. In an `index.ts` file: a. Import your `deriveKey` function and your `salt`. b. Choose a simple, weak password (e.g., "football"). c. Call `deriveKey` with the weak password and the imported salt. d. Log the original password, the salt, and the final derived key to the console to see the transformation.