# 💡 DK04.3.4: Praktik Terbaik dengan .env files

**Outline:**

- **The Best Friend (SEEI):** Using `.env` files as the local source of truth for configuration.
- **The Safety Net (PPP):** Implementing `.gitignore`, `.env.example`, and validation.
- **Your Mission (Production):** Standardizing a project's environment setup for a new team member.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Best Friend (SEEI)**

**Why `.env`?**
It provides a standardized way to store configuration that is separate from your code. It's the physical manifestation of "Separation of Concerns."

**The Golden Rules of `.env`:**

1. **LOCAL ONLY:** Never check `.env` into version control (Git).
2. **FLAT:** Use simple `KEY=VALUE` pairs.
3. **DOCUMENTED:** If you add a variable to `.env`, add a dummy version to `.env.example`.

### **Part 2: Practice - The Safety Net (PPP)**

**The "Starter Pack" for every project:**

1. **`.env`**: Contains your real secrets (DB_PASS=123). **(GITIGNORED)**
2. **`.env.example`**: Contains placeholders (DB_PASS=your_password_here). **(COMMITTED TO GIT)**
3. **`.gitignore`**: Must contain the line `.env`.

**The Onboarding Workflow:**
A new developer joins the team:

- They clone the repo.
- They see `.env.example`.
- They run `cp .env.example .env`.
- They fill in their own local values.
- They run `docker compose up`.

## 🧠 **Real-World Case Study: "The Missing Variable"**

- **Scenario:** A senior developer adds a new "Stripe API Key" requirement to the app and updates their local `.env`.
- **The Problem:** They forget to update `.env.example`.
- **The Result:** The junior developer pulls the code, starts the app, and gets a cryptic `NullPointerException`. They spend 3 hours debugging code before realizing they just lacked a configuration variable.
- **The Fix:** The team implements a "CI/CD check" that fails if `.env.example` doesn't match the keys in the production environment.
- **The Lesson:** Your `.env.example` is your contract with other developers. Keep it up to date.

## 🤔 **Reflective Questions**

1. Why shouldn't you use quotes around values in a `.env` file unless they contain spaces?
2. What happens if a variable is in your Shell AND in your `.env` file? (Answer: Usually, the Shell wins).
3. Can you have separate `.env` files for different environments (e.g., `.env.prod`, `.env.dev`)?

## 📚 **Further Reading**

- **Guide:** [The 12-Factor App - Config](https://12factor.net/config)
- **Tool:** [dotenv linter](https://dotenv-linter.github.io/)

## 📝 **Mini Task (Production)**

1. Look at your current Docker project.
2. Do you have a `.env` file?
3. Is it in your `.gitignore`? (Check it NOW with `git check-ignore .env`).
4. Create a `.env.example` file that lists every variable needed for your app to start.
5. Write a one-line README instruction for how a new developer should use these files.
6. Bonus: Use a "Generic" value in the example file so it's clear what the format of the key should be (e.g., `API_KEY=sk_test_...`).
