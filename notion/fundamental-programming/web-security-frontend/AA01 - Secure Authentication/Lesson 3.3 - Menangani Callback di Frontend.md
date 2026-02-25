💡 AA01-3.3: Handling the Frontend Callback

**Outline:**

- **The Threat (SEEI):** Understanding what the frontend should _not_ do with the authorization code it receives.
- **The Defense (PPP):** Implementing the `onSuccess` handler to securely relay the code to the backend.
- **Your Mission (Production):** Time to connect your login button to your own backend API.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Improper Handling of the Authorization Code.** After the user successfully logs in via the Google popup, the frontend receives a short-lived, one-time-use Authorization Code. The primary threat is a misunderstanding of the frontend's role. If a developer tries to use this code on the client-side to fetch user data directly from Google, they would have to embed the application's `client_secret` in the JavaScript code. This is a critical security failure. **A `client_secret` must _never_ be exposed in client-side code.** It's like embedding your database password directly in your HTML.
    
- **How it Works (The Attack Vector / Catastrophic Pattern):** A developer misunderstands the flow and tries to complete the OAuth exchange on the client.
    
    ```ts
    // CATASTROPHICALLY INSECURE CODE - DO NOT EVER DO THIS
    import { GoogleLogin } from '@react-oauth/google';
    
    const MyLoginComponent = () => {
      // This secret is now visible to anyone who views the page source.
      const CLIENT_SECRET = 'super_secret_string_from_google_console';
    
      const handleSuccess = async (codeResponse) => {
        // The developer tries to exchange the code for a token on the client.
        const response = await fetch('<https://oauth2.googleapis.com/token>', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            code: codeResponse.code,
            client_id: 'YOUR_CLIENT_ID',
            client_secret: CLIENT_SECRET, // <-- HUGE VULNERABILITY
            redirect_uri: 'postmessage',
            grant_type: 'authorization_code'
          })
        });
        // An attacker has already stolen your client_secret and can now
        // impersonate your application.
      };
    
      return <GoogleLogin onSuccess={handleSuccess} flow="auth-code" />;
    }
    
    ```
    
    Once your `client_secret` is public, an attacker can impersonate your application, potentially tricking users into granting permissions to the attacker's malicious services. You would have to immediately revoke the secret and get a new one.
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **The frontend's only job is to act as a secure courier.** It receives the authorization code from the provider (Google) and immediately passes it to its own trusted backend in a single, secure transaction. It should not inspect, modify, or try to use the code for any other purpose.
    
- **Secure Code Implementation:** We will implement the `onSuccess` handler correctly. It will take the code and make a POST request to our own API.
    
    ```ts
    // App.tsx - The Secure Way
    
    import { GoogleLogin, useGoogleLogin } from '@react-oauth/google';
    import axios from 'axios'; // Using Axios for API calls
    
    function App() {
      // Note: The @react-oauth/google library has a helpful hook `useGoogleLogin`
      // for more control than the simple button component.
    
      const login = useGoogleLogin({
        // This is the correct "Authorization Code" flow
        flow: 'auth-code',
    
        // This function is our secure callback handler
        onSuccess: async (codeResponse) => {
          console.log('Received auth code:', codeResponse.code);
          try {
            // Send the one-time-use authorization code to our backend
            const response = await axios.post('/api/auth/google', {
              code: codeResponse.code,
            });
    
            // Our backend will respond with user data and set session cookies
            console.log('Backend response:', response.data);
    
            // Now, we can redirect the user or update the UI state
            window.location.href = '/dashboard';
    
          } catch (error) {
            console.error('Login failed. Server error:', error);
          }
        },
        onError: (errorResponse) => {
          console.error('Google login error:', errorResponse);
        },
      });
    
      return (
        <div>
          <h1>My App</h1>
          <button onClick={() => login()}>
            Sign in with Google 🚀
          </button>
        </div>
      );
    }
    
    export default App;
    
    ```
    
    This approach maintains a clean separation of concerns. The frontend handles the user interaction, and the backend handles the secure, credentialed communication with the OAuth provider.
    

### 🧠 Real-World Case Study: "The Signed Letter of Introduction"

- **The Goal:** You want to prove your identity to a secure government building (your backend) using a letter from a trusted embassy (Google).
- **Vulnerable Approach (Client-Side Exchange):** The embassy gives you a signed, sealed letter of introduction (`authorization_code`). You then try to find the government's secret codebook (`client_secret`) which is lying around in the public park (`client-side JS`), open the letter yourself, and try to authenticate. This is suspicious and insecure.
- **Secure Approach (Backend Exchange):** The embassy gives you the signed, sealed letter (`authorization_code`). You, the courier (`frontend`), immediately take this unopened letter to the front desk of the government building (`your backend API endpoint`). The guard at the desk (`backend server`) takes the letter, goes into a secure back room, and uses their own secret codebook (`client_secret`) to verify the letter's authenticity directly with the embassy. Once verified, they issue you an official building access pass (`your application's JWT/session`).

### 🤔 Reflective Questions

1. Why is the authorization code from Google typically a one-time-use code? What attack does this prevent?
2. In a real application, what should the UI do while it's waiting for the response from your backend after sending the code? (e.g., show a spinner, disable the login button).
3. The `useGoogleLogin` hook gives more control than the `<GoogleLogin />` component. What are some advantages of using the hook-based approach for a complex application?

### 📚 Further Reading

- **@react-oauth/google Docs:** [useGoogleLogin hook](https://www.google.com/search?q=https://www.npmjs.com/package/%40react-oauth/google%23usegooglelogin-hook)
- **Axios Docs:** [Making POST Requests](https://www.google.com/search?q=https://axios-http.com/docs/post_requests)
- **Google's Docs:** [The server-side flow (see Step 5)](https://www.google.com/search?q=https://developers.google.com/identity/protocols/oauth2/web-server%23handlingresponse)

### 📝 Mini-Task (Production)

Building on the previous lesson's task where you displayed a Google Login button, your new mission is to implement the `onSuccess` callback handler correctly.

1. Refactor your code to use the `useGoogleLogin` hook with the `flow: 'auth-code'` option. Create a custom button that calls the `login()` function returned by the hook.
2. Inside the `onSuccess` handler, perform the following actions:
    - Log the received `codeResponse.code` to the browser's console.
    - Use `fetch` or `axios` to make a `POST` request to a _mock_ backend endpoint: `/api/v1/auth/google/callback`.
    - Include the received code in the body of the POST request, like so: `{ "code": "THE_CODE_FROM_GOOGLE" }`.
    - You don't need a real backend yet. The network request will fail with a 404, which is expected. The goal is to correctly implement the frontend part of the communication.