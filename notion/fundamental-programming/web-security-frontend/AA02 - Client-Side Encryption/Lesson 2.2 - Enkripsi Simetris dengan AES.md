💡 AA02-2.2: Locking the Data: Symmetric Encryption with AES

**Outline:**

- **The Threat (SEEI):** A user's private data (like a diary entry or a personal note) is stored as plaintext, making it visible to anyone with database access.
- **The Defense (PPP):** Using the AES (Advanced Encryption Standard) algorithm from CryptoJS to encrypt the plaintext data into unreadable ciphertext before storage.
- **Your Mission (Production):** Time to write a function that takes plaintext and a secret key, and returns AES-encrypted ciphertext.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Plaintext at Rest.** In Lesson 1.4, we discussed the architectural risk of a server breach. This is the implementation-level detail of that risk. When you design a feature like a "personal notes" section in an application, the most straightforward way to build it is to take the user's text and save it directly into a database column. The data is now "at rest" on the server. If an attacker breaches that database, they can read every user's personal notes. The confidentiality of the data is completely dependent on the security of the server.
- **How it Works (The Attack Vector):** A user writes a sensitive note: "My bank account password is `T0pS3cr3t!`". The application saves this string directly into the `notes` table in the database. Later, an attacker uses a SQL injection vulnerability to run `SELECT * FROM notes`. The attacker now has the plaintext of every note, including the user's bank password.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Encrypt sensitive data before it leaves the client.** We will use AES, the industry-standard algorithm for symmetric encryption. It's a "block cipher," meaning it encrypts data in fixed-size blocks. It is fast, secure, and globally recognized.
    
- **Secure Code Implementation:** Let's build an encryption function.
    
    ```ts
    import { AES, enc } from 'crypto-js';
    
    /**
     * Encrypts a plaintext string using AES with a given secret key.
     * @param plaintext The string data to encrypt.
     * @param secretKey The secret key to use for encryption.
     * @returns The Base64-encoded ciphertext.
     */
    function encryptData(plaintext: string, secretKey: string): string {
      if (!plaintext || !secretKey) {
        throw new Error("Plaintext and secret key are required.");
      }
    
      // The encrypt function returns a special CipherParams object.
      const encrypted = AES.encrypt(plaintext, secretKey);
    
      // We convert the CipherParams object to a string.
      // Base64 is a common, safe format for transmitting or storing ciphertext.
      return encrypted.toString();
    }
    
    // --- USAGE ---
    const myNote = "This is a very secret note about my project ideas.";
    const mySecret = "my-super-secret-password-or-key";
    
    try {
      const ciphertext = encryptData(myNote, mySecret);
      console.log("Plaintext:", myNote);
      console.log("Ciphertext:", ciphertext);
      // Example output might start with: "U2FsdGVkX1+..."
    
      // This ciphertext is what you would send to the server to be stored.
    } catch (error) {
      console.error("Encryption failed:", error);
    }
    
    ```
    
    The output string `ciphertext` is a self-contained package that includes the encrypted data as well as the salt and initialization vector (IV) needed for decryption later. It's safe to store and transmit.
    

### 🧠 Real-World Case Study: "The Pig Latin Message"

- **The Goal:** Two kids want to pass a secret note in class.
- **The System:** They agree on a secret rule (`the key`): "To encrypt, move the first letter of each word to the end and add 'ay'". This is their `algorithm`.
- **Vulnerable Approach (Plaintext):** The note says `MEET AT THE PLAYGROUND`. If the teacher intercepts it, the plan is revealed.
- **Secure Approach (Encryption):**
    - **Plaintext:** `MEET AT THE PLAYGROUND`
    - **Key:** The secret rule.
    - **Encryption Process:**
        - MEET -> EETM -> EETM`ay`
        - AT -> T`a` -> T`a`ay
        - THE -> HET -> HET`ay`
        - PLAYGROUND -> LAYGROUNDP -> LAYGROUNDP`ay`
    - **Ciphertext:** `EETMay ATay ETHay AYGROUNDPlay` The teacher intercepts the note but just sees gibberish. Only the other child, who knows the secret rule (`the key`), can reverse the process to read the message. AES is just a vastly more complex and secure version of this concept.

### 🤔 Reflective Questions

1. The `AES.encrypt()` function returns an object. Why is it useful to convert this to a single string (`.toString()`) for storage? What information do you think is contained within that string besides the raw encrypted data?
2. In the example, we used a human-readable string as the `secretKey`. Is this a secure practice? What are the potential weaknesses of using a simple password directly as a key? (This is a preview of Lesson 2.4).
3. AES comes in different key sizes (128-bit, 192-bit, 256-bit). What is the trade-off between using a larger key size versus a smaller one?

### 📚 Further Reading

- **CryptoJS on GitHub:** [AES Usage Examples](https://www.google.com/search?q=https://github.com/brix/crypto-js%23aes-encryption)
- **[NIST.gov](http://NIST.gov):** [The Official AES Standard Information](https://www.google.com/search?q=https://csrc.nist.gov/projects/advanced-encryption-standard-aes)
- **Wikipedia:** [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)

### 📝 Mini-Task (Production)

Your task is to create a simple encryption utility.

1. Create a file named `encryptor.ts`.
2. Inside this file, write the `encryptData(plaintext: string, secretKey: string): string` function exactly as shown in the lesson.
3. Export the function.
4. In a separate file, `index.ts`, import your `encryptData` function.
5. Define a variable containing a secret message of your choice and another variable for a secret key.
6. Call your function with these variables and log the resulting ciphertext to the console.