💡 AA01-3.1: An Introduction to OAuth 2.0

**Outline:**

- **The Threat (SEEI):** Understanding the immense risk and liability of storing user passwords.
- **The Defense (PPP):** Introducing OAuth 2.0 as a protocol for delegated authorization, not authentication.
- **Your Mission (Production):** Time to diagram the most common and secure OAuth 2.0 flow.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Direct Password Handling.** As an application developer, the most toxic asset you can possess is a user's password. Storing passwords, even when securely hashed, makes your database a prime target for attackers. A breach means you are responsible for a credential that the user might have reused on other services. This leads to massive liability, loss of user trust, and regulatory nightmares (like GDPR). You have to worry about hashing algorithms, salt management, and constant security audits. It's a high-stakes game you don't want to play if you don't have to.
- **How it Works (The Attack Vector):**
    1. Your application stores usernames and hashed passwords in its database.
    2. An attacker finds a SQL injection vulnerability or another flaw and dumps your entire user table.
    3. Even with hashing, the attacker can run the hashes through offline "rainbow tables" or brute-force attacks to crack weaker passwords.
    4. The attacker now has the cleartext passwords for a subset of your users. They will try these same email/password combinations on high-value targets like email providers, banks, and social media (this is called "credential stuffing").
    5. Your application's breach has now become a security incident for your users across the entire internet.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Never handle passwords you don't have to.** OAuth 2.0 is a security standard that allows a user to grant a third-party application limited access to their resources on another service, _without_ sharing their credentials. It's a protocol for **delegated authorization**. The user tells Google, "Hey, I trust this React app to see my name and email address." Google then gives the app a token that represents that specific permission. The app never sees the user's Google password.
    
- **Secure Code Implementation (The "Authorization Code" Flow):** This is the most secure and common flow for web applications. Let's identify the actors:
    
    - **Resource Owner:** The User.
    - **Client:** Your React Application.
    - **Authorization Server:** The service the user trusts (e.g., Google's login server).
    - **Resource Server:** The API that holds the user's data (e.g., Google's Profile API).
    
    The flow works like this:
    
    1. **User Clicks "Login with Google"**: Your React app redirects the user to Google's login page with your app's `client_id`.
    2. **User Authenticates & Consents**: The user enters their Google password *on [https://www.google.com/search?q=google.com*](https://www.google.com/search?q=google.com*) (not your site) and approves the permissions your app is requesting.
    3. **Google Redirects Back with a Code**: Google redirects the user back to your specified "callback URL" with a temporary, one-time-use **Authorization Code** in the URL parameters.
    4. **Frontend Sends Code to Backend**: Your React app's frontend code grabs this code and immediately sends it to your application's backend server.
    5. **Backend Exchanges Code for Tokens**: Your backend server securely communicates with Google's backend, exchanging the authorization code, its `client_id`, and a `client_secret` for an **Access Token**.
    6. **Backend Creates a Session**: The backend uses the access token to ask Google for the user's profile info, then creates a local session for the user (e.g., by issuing its own JWT) and sends it back to the React app.

### 🧠 Real-World Case Study: "The Valet Key"

- **The Goal:** Let a valet park your car without giving them full access to everything.
- **Vulnerable Approach (Direct Password Handling):** You give the valet the master key to your car. This key not only starts the ignition but also opens the trunk and the glove compartment where you keep your valuables. You are trusting the valet completely not to snoop.
- **Secure Approach (OAuth 2.0 Authorization Code Flow):** You give the valet a special **Valet Key** (`Access Token`). This key is limited: it can only start the car and drive it for a short distance (`scope`). It cannot open the trunk or the glove compartment. You, the car owner, have _delegated_ the specific permission of "parking the car" to the valet, without giving them the keys to your entire life.

### 🤔 Reflective Questions

1. OAuth 2.0 is a framework for authorization, not authentication. What's the difference? (Hint: Authorization is about what you're allowed to _do_, authentication is about proving who you _are_).
2. In the Authorization Code flow, why is it critical that the final step (exchanging the code for a token) happens on the backend server and not the frontend?
3. What is the purpose of the `scope` parameter in an OAuth 2.0 request? How does it relate to the principle of least privilege?

### 📚 Further Reading

- **OAuth 2.0 Simplified:** [A clear, easy-to-understand guide](https://oauth.net/2/)
- **IETF RFC 6749:** [The Official OAuth 2.0 Specification](https://datatracker.ietf.org/doc/html/rfc6749)
- **Google's OAuth 2.0 Docs:** [Using OAuth 2.0 for Web Server Applications](https://developers.google.com/identity/protocols/oauth2/web-server)

### 📝 Mini-Task (Production)

You are designing a new social media scheduling app ("PostBot") built in React. Users will log in with their Twitter account to allow PostBot to schedule tweets on their behalf.

Your task: Diagram the OAuth 2.0 Authorization Code flow for this scenario. Draw a sequence diagram or write a numbered list identifying the four main actors (Resource Owner, Client, Authorization Server, Resource Server) and describing each of the 6 main steps in the flow, from the user clicking "Login with Twitter" to PostBot's backend being ready to post a tweet.