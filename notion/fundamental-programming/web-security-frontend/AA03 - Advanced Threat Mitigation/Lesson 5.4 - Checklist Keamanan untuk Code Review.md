💡 AE01-5.4: The Reviewer's Shield: A Security Code Review Checklist

**Outline:**

- **The Threat (SEEI):** Code reviews that focus only on logic, style, and performance can easily miss common security flaws. Without a systematic process, security becomes an afterthought.
- **The Defense (PPP):** Integrating a standardized, lightweight security checklist into every frontend pull request. This turns code reviews into a consistent security gate and an educational tool for the whole team.
- **Your Mission (Production):** Time to use a security checklist to analyze a hypothetical pull request and spot the vulnerabilities.

### 📘 Full Lesson Content

### **Part 1: Presentation - The Threat (SEEI)**

- **What is it? (The "Vulnerability"):** **Ad-hoc Security Reviews.** When there's no formal process for security code reviews, vulnerabilities are missed for several reasons:
    - **Lack of Knowledge:** Reviewers may not be trained to spot security issues.
    - **Inconsistency:** One reviewer might check for XSS, another might not. The level of scrutiny depends entirely on who is assigned the PR.
    - **"Rubber Stamping":** For urgent features, reviewers might quickly approve a PR after only a superficial glance, assuming someone else has considered the security implications. This leads to a culture where security is not a shared responsibility, and vulnerabilities inevitably slip through to production.
- **How it Works (The Attack Vector):**
    1. A junior developer submits a pull request for a new feature. The code uses `dangerouslySetInnerHTML` with data from a URL parameter.
    2. The senior developer assigned to review the PR is busy. They see the code works according to the feature requirements, the style is correct, and the tests pass. They focus on a minor performance suggestion and approve the PR.
    3. Neither developer explicitly thought to check for DOM-based XSS because it wasn't part of their standard review process.
    4. The vulnerable code is deployed.

### **Part 2: Practice - The Defense (PPP)**

- **The Golden Rule:** **Make security a non-negotiable part of the definition of "done."** A pull request is not ready to merge until it has been explicitly checked against a standard set of security controls.
    
- **Secure Code Implementation (The Checklist):** The best implementation is not code, but a documented process. This checklist should be added to your team's pull request template on GitHub/GitLab.
    
    ### 🛡️ Frontend Security Review Checklist
    
    _(Reviewer: Please check each box or comment on why it's not applicable)_
    
    **1. Input Handling & XSS**
    
    - [ ] Is all user-controllable data (from APIs, URL params, user input) treated as untrusted?
    - [ ] Does this PR avoid `dangerouslySetInnerHTML`? If not, is the input guaranteed to be sanitized with a library like DOMPurify?
    - [ ] Are any URLs being used in `<a>` tags or `window.location` validated to prevent `javascript:` XSS?
    
    **2. Component & API Design**
    
    - [ ] If new components are added, are their APIs "secure by default"? (i.e., they don't encourage passing in raw, unsafe data).
    - [ ] Are sensitive actions (e.g., changing email, deleting data) performed via `POST` requests and not `GET` requests? (CSRF Prevention)
    
    **3. Dependencies & Supply Chain**
    
    - [ ] If a new third-party dependency is added (`package.json`), has it been vetted for security and necessity?
    - [ ] If a new script is loaded from a CDN, does it use Subresource Integrity (SRI)?
    - [ ] If an `iframe` is added, does it use the `sandbox` attribute with the minimum required permissions?
    
    **4. Information Exposure**
    
    - [ ] Are we avoiding logging sensitive user data or tokens to the console in production builds?
    - [ ] Does this change expose any new, sensitive data to the client that shouldn't be there?

### **Part 3: Production - Using the Checklist**

By integrating this into the PR template, you create a constant reminder. It forces both the author and the reviewer to pause and consider the security implications of the code. Over time, this builds a shared security consciousness within the team. Developers will start writing more secure code from the beginning because they know it will be checked for during the review.

### 🧠 Real-World Case Study: "The PR Template"

- **The Goal:** A fast-growing startup wants to scale its engineering team without scaling its security vulnerabilities.
- **The System:** They modify their default Pull Request template on GitHub.
- **Before (The Vulnerable Process):**
    - The PR description is a blank text box.
    - Reviews are informal. Features are merged quickly.
    - The security team periodically finds vulnerabilities in production, leading to fire-drills and delays.
- **After (The Secure Process):**
    - Every new PR is pre-populated with a template that includes the security checklist.
    - The author is encouraged to review their own code against the checklist before requesting a review.
    - The reviewer is required to go through the checklist.
    - **Result:** The number of simple vulnerabilities reaching production drops significantly. The checklist becomes an educational tool, leveling up the security skills of the entire team. The process shifts from reactive (finding bugs in production) to proactive (preventing them at the source).

### 🤔 Reflective Questions

1. What is the risk of making a security checklist _too_ long and detailed? How do you find the right balance between being thorough and being practical?
2. How can you automate parts of this security review? (Hint: Think about linters, static analysis tools, and CI/CD pipeline checks).
3. This checklist is for the frontend. What are some items you would expect to see on a _backend_ security code review checklist?

### 📚 Further Reading

- **OWASP:** [Code Review Guide](https://owasp.org/www-project-code-review-guide/) (Very comprehensive and detailed).
- **GitHub Docs:** [Creating a pull request template for your repository](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)
- **Snyk:** [Secure coding checklist](https://www.google.com/search?q=https://snyk.io/learn/secure-coding/secure-coding-checklist/)

### 📝 Mini-Task (Production)

You are reviewing a small pull request that adds a "user profile" page. The code includes the following snippets:

- **Snippet 1 (ProfileBio.jsx):**`const bioHtml = convertMarkdownToHtml(user.bio);return <div dangerouslySetInnerHTML={{ __html: bioHtml }} />;`
- **Snippet 2 (WebsiteLink.jsx):**`return <a href={user.websiteUrl}>Visit my website</a>;`
- **Snippet 3 (package.json diff):**`+ "left-pad": "^1.3.0",`

Your mission: Use the security checklist provided in this lesson to review this PR. Identify at least three distinct potential security issues. For each issue, state which checklist item it violates.