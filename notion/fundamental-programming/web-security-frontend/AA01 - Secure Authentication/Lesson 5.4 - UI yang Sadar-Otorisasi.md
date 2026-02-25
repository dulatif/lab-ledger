💡 AA01-5.4: Authorization-Aware UI

**Outline:**

- **The Threat (SEEI):** Understanding the poor UX and potential information leakage from showing UI elements to users who aren't authorized to use them.
- **The Defense (PPP):** Implementing logic to conditionally render components based on user roles and permissions, ensuring the UI adapts to the user's capabilities.
- **Your Mission (Production):** Time to build a component that conditionally renders its children based on a user's role.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The Vulnerability):** **Leaking Features & Frustrating Users.** This threat moves beyond simple _authentication_ (Are you logged in?) to _authorization_ (What are you allowed to _do_?). If your UI shows a "Delete" button to a user who doesn't have deletion privileges, two problems arise:
    
    1. **Bad User Experience:** The user clicks the button, and the API call predictably fails with a `403 Forbidden` error. This is frustrating and makes the application feel broken.
    2. **Information Leakage:** Showing an "Admin Panel" button to a non-admin user, even if the route is protected, tells them that an admin panel exists. This gives attackers a target to focus on. A truly secure UI should not even hint at the existence of features a user is not authorized to access.
- **How it Works (The Bad Pattern):** The backend correctly protects the API, but the frontend doesn't adapt the UI.
    
    ```tsx
    // BAD PATTERN: UI DOES NOT RESPECT PERMISSIONS
    
    // Post.tsx
    // The user object we get from the API includes roles.
    // user = { name: 'Bob', roles: ['viewer'] }
    const Post = ({ post }) => {
    
      const handleDelete = async () => {
        try {
          // This API call will fail with a 403 Forbidden for a 'viewer'.
          await apiClient.delete(`/posts/${post.id}`);
        } catch (error) {
          alert("Error: You don't have permission to do that."); // Bad UX!
        }
      };
    
      return (
        <div>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
          {/* This button is shown to everyone, even users who can't use it. */}
          <button onClick={handleDelete}>Delete Post</button>
        </div>
      );
    };
    
    ```
    

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **UI elements should reflect API permissions.** If a user cannot perform an action on the backend, they should not see the UI control for that action on the frontend. This is the principle of least privilege applied to the user interface. **Frontend authorization is a UX enhancement; backend authorization is the real security.**
- **Secure Code Implementation:** We can create a simple component that conditionally renders its children based on the current user's roles.
    1. **First, ensure your `useAuth` hook exposes user roles:**
        
        ```tsx
        // hooks/useAuth.ts (modification)
        export const useAuth = () => {
          const user = useAuthStore((state) => state.user);
          // ...
          return useMemo(() => ({
            user,
            isAuthenticated: !!user,
            roles: user?.roles || [], // Expose the user's roles
            // ...
          }), [user]);
        };
        
        ```
        
    2. **Create a role-checking component:**
        
        ```tsx
        // components/Authorize.tsx
        import { useAuth } from '../hooks/useAuth';
        
        // This component will render its children only if the user has
        // at least one of the roles in the `allowedRoles` array.
        const Authorize = ({ allowedRoles, children }) => {
          const { roles } = useAuth();
        
          const isAuthorized = roles.some(role => allowedRoles.includes(role));
        
          return isAuthorized ? children : null;
        };
        
        export default Authorize;
        
        ```
        
    3. **Use it in your UI:**
        
        ```tsx
        // Post.tsx (Secure and good UX)
        import Authorize from './components/Authorize';
        
        const Post = ({ post }) => {
          const handleDelete = () => { /* ... */ };
        
          return (
            <div>
              <h2>{post.title}</h2>
              <p>{post.content}</p>
        
              {/* This button will ONLY be rendered if the user is an 'editor' or 'admin'. */}
              <Authorize allowedRoles={['editor', 'admin']}>
                <button onClick={handleDelete}>Delete Post</button>
              </Authorize>
            </div>
          );
        };
        
        ```
        

### 🧠 Real-World Case Study: "The Elevator Keycard System"

- **The Goal:** Control access to different floors of a secure building.
- **Vulnerable Approach (Backend Auth Only):** The elevator has buttons for every floor, including the top-secret "Floor 13". A regular employee can press the "13" button (`visible UI`). The light on the button even turns on. However, when the doors close, the elevator's internal control system (`backend API`) checks their keycard's permissions, sees they aren't authorized, and refuses to move. The employee is confused and frustrated.
- **Secure Approach (UI + Backend Auth):** The elevator uses a smart panel (`authorization-aware UI`). When a regular employee scans their keycard, the panel _only_ shows them buttons for the floors they are allowed to visit (e.g., Lobby, Floor 7). The button for Floor 13 is not visible at all. When the CEO scans their keycard, the button for Floor 13 appears. The UI adapts to the user's permissions, providing a seamless experience and not revealing the existence of inaccessible floors.

### 🤔 Reflective Questions

1. Why is it still critically important to enforce authorization on the backend API, even if you have a perfectly implemented authorization-aware UI on the frontend?
2. The `<Authorize>` component hides content. What if you wanted to disable a button instead of hiding it? How would you modify the component to support both behaviors?
3. How can this pattern of declarative authorization simplify your component logic and make your code easier to read and test?

### 📚 Further Reading

- **OWASP:** [A01:2021 – Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) (The #1 web application security risk).
- **Blog Post:** [Role-Based Access Control (RBAC) in React](https://www.google.com/search?q=https://www.osohq.com/post/role-based-access-control-react)
- **CanCanCan:** [A popular authorization library for Ruby on Rails](https://github.com/CanCanCommunity/cancancan) (To see how these concepts are applied in other frameworks).

### 📝 Mini-Task (Production)

You are building the UI for a blog. Your `useAuth` hook returns a `user` object that can look like this: `{ id: '123', name: 'Bob', roles: ['author'] }` or `{ id: '456', name: 'Alice', roles: ['editor', 'admin'] }`.

Your task: Create a `<PostActions />` component. This component should display:

1. An "Edit Post" button, visible to users with either the `author` or `editor` role.
2. A "Publish Post" button, visible _only_ to users with the `editor` role.
3. A "Delete Post" button, visible _only_ to users with the `admin` role.

Use the conditional rendering pattern (`isAuthorized && <Component />`) or the `<Authorize>` component from the lesson to implement this logic.