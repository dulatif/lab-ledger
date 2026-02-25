💡 AA02-1.3: One-Way Ticket: The Difference Between Hashing and Encryption

**Outline:**

- **The Threat (SEEI):** The risk of using a reversible process (encryption) to store data that should never be revealed, like user passwords.
- **The Defense (PPP):** Introducing hashing as a one-way function for data integrity and password storage, and contrasting its purpose with two-way encryption for data confidentiality.
- **Your Mission (Production):** Time to decide where to use hashing and where to use encryption in an application's data flow.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Reversible Password Storage.** Imagine you store user passwords in your database by encrypting them with a secret key. This seems secure, right? But what happens if an attacker compromises your application server and steals that encryption key? They can now decrypt your entire password database and have the cleartext passwords for all your users. Encryption is a two-way street; any data that can be encrypted can be decrypted with the right key. For data like passwords, there is almost never a legitimate reason for your application to need to know the original plaintext version. Storing it in a reversible format is an unnecessary risk.
- **How it Works (The Attack Vector):**
    1. A web application encrypts user passwords with a key that is stored in a server configuration file.
    2. An attacker exploits a vulnerability (e.g., path traversal) to read the configuration file and steal the encryption key.
    3. The attacker then performs a SQL injection to dump the user table, which contains the encrypted passwords.
    4. The attacker takes the stolen data offline and uses the stolen key to decrypt every password. Game over.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Encrypt what you need to get back; hash what you only need to verify.**
    
    - Use **Encryption** for **Confidentiality**: Keeping secret data secret, with the intention of revealing it later to an authorized party (e.g., a chat message, a private note). It's a **two-way** function.
    - Use **Hashing** for **Integrity & Verification**: Proving that data hasn't been tampered with or verifying that a piece of input (like a password) matches a stored value without ever storing the value itself. It's a **one-way** function.
- **Secure Code Implementation (Conceptual):**
    
    **Hashing:** A hash function takes an input of any size and produces a fixed-size string of characters, called a hash or digest.
    
    - **Deterministic:** The same input will always produce the same output.
    - **One-Way:** It is computationally infeasible to go from the output (hash) back to the original input.
    - **Collision Resistant:** It's extremely difficult to find two different inputs that produce the same output hash.
    - **Algorithms:** SHA-256, SHA-3, bcrypt, Argon2 (bcrypt and Argon2 are specifically designed for passwords).
    
    ```ts
    // HASHING - A ONE-WAY TRIP
    // Used for password storage and integrity checks.
    
    // Storing a password:
    const userPassword = "MyStrongPassword123";
    // IMPORTANT: We use a "salt" (a random string) to make the hash unique
    // even if two users have the same password.
    const salt = generateSalt();
    const passwordHash = hash(userPassword + salt); // e.g., using bcrypt
    
    // Store `passwordHash` and `salt` in the database. NEVER the plaintext password.
    
    // Verifying a password during login:
    const loginAttempt = "MyStrongPassword123";
    const storedHash = "..."; // from DB
    const storedSalt = "..."; // from DB
    
    // Hash the login attempt with the SAME salt.
    const attemptHash = hash(loginAttempt + storedSalt);
    
    // If the hashes match, the password is correct.
    // We never had to decrypt or see the original password.
    if (attemptHash === storedHash) {
      // Login successful!
    }
    
    ```
    

### 🧠 Real-World Case Study: "The Restaurant Order vs. The Allergy Check"

- **The Goal:** A restaurant needs to handle customer food orders and check for allergies.
- **Encryption (Confidentiality):** You write your order on a piece of paper (`plaintext`) and put it in a sealed, locked box (`encryption`). You give the box to the waiter. The waiter takes it to the chef, who has the only key (`decryption key`). The chef unlocks the box, reads the order (`decrypted plaintext`), and makes your food. The process needed to be **two-way** to be useful.
- **Hashing (Integrity/Verification):** You have a severe peanut allergy. When you signed up for the restaurant's loyalty program, they asked for your allergies. They didn't write down "peanuts"; instead, they put the word "peanuts" through a powerful shredder (`hashing`). They stored the pile of shredded paper (`the hash`). Now, when you order the "Spicy Thai Noodles", the system checks the ingredients. It sees "peanuts". It runs "peanuts" through the _same shredder algorithm_ and compares the new pile of shreds to the one they have on file for you. The piles match. The system can confirm "This dish contains an allergen for this customer" without ever having to store the word "peanuts" in a readable format. It's a **one-way** verification.

### 🤔 Reflective Questions

1. What is a "salt" in the context of hashing passwords, and what specific type of attack does it prevent? (Hint: Think about what happens if 1,000 users all choose the password "password123").
2. File integrity is a common use case for hashing. How could you use a SHA-256 hash to verify that a large software file you downloaded wasn't corrupted or tampered with?
3. Why is an algorithm like MD5 no longer considered secure for hashing?

### 📚 Further Reading

- **OWASP:** [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- **Cracking Passwords with Rainbow Tables (YouTube):** [Computerphile video explaining the attack that salts prevent](https://www.google.com/search?q=https://www.youtube.com/watch%3Fv%3DFRZv83b93j4)
- **Toptal:** [A Plain English Introduction to Hashing](https://www.google.com/search?q=https://www.toptal.com/security/a-plain-english-introduction-to-hashing)

### 📝 Mini-Task (Production)

You are designing the data handling for a new web application. For each piece of data below, decide if you should **hash** it or **encrypt** it when storing it on your server. Explain your choice in one sentence.

1. A user's chosen password for their account.
2. A private diary entry that the user writes and saves, which they need to be able to read and edit later.
3. The contents of a user's shopping cart before they check out.
4. A user's date of birth, which your application needs to use to wish them a happy birthday.