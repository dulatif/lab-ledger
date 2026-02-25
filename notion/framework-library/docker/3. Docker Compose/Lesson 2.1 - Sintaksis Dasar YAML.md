# 💡 DK03.2.1: Sintaksis Dasar YAML

**Outline:**

- **The Rules (SEEI):** Mastering the nuances of YAML for Docker Compose.
- **The Data Structures (PPP):** Understanding Scalars, Mappings, and Lists.
- **Your Mission (Production):** Fixing a "Broken" YAML file.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Rules (SEEI)**

**YAML is NOT Code**
Unlike Python or JavaScript, YAML is a "Data Serialization Language." It doesn't perform logic; it simply represents data in a highly structured way.

**The Golden Rules of YAML:**

1. **Case Sensitivity:** `Image` is NOT the same as `image`.
2. **Indentation Matters:** You must use consistent spaces (usually 2).
3. **Comments:** Use `#` for documentation.
4. **Key-Value Pairs:** Separated by a colon and a SPACE (`key: value`).

**The Importance of the SPACE:**

- `image:nginx` (Wrong! Missing space after colon).
- `image: nginx` (Correct).

### **Part 2: Practice - The Data Structures (PPP)**

**The Three Main Types in Compose:**

1. **Mapping (Objects):** Key-value pairs.
   ```yaml
   build:
     context: .
     dockerfile: Dockerfile
   ```
2. **Lists (Sequences):** Start with a dash `-`.
   ```yaml
   ports:
     - "3000:3000"
     - "8080:80"
   ```
3. **Scalars:** Simple values (Strings, Booleans, Numbers).
   ```yaml
   restart: always
   privileged: true
   ```

**The Quote Rule:**
Always prefer quotes around port mappings (`"80:80"`) and version numbers (`"3.8"`) to prevent YAML from interpreting them as numbers or times.

## 🧠 **Real-World Case Study: "The Secret Map"**

- **Scenario:** A developer tries to set a password in YAML: `password: 12345`.
- **The Problem:** Depending on the YAML parser, `12345` might be treated as a number, which could cause issues if the application expects a string. Even worse, if the password starts with a zero (`012345`), YAML might interpret it as an **Octal** number!
- **The Solution:** Always wrap strings in quotes: `password: "012345"`.
- **The Lesson:** Be explicit. Quotes are your best friend for avoiding "Magic Type" bugs.

## 🤔 **Reflective Questions**

1. Why are TABS forbidden in most YAML parsers?
2. What happens if you forget the space after a dash in a list?
3. Is it possible to have nested lists in YAML?

## 📚 **Further Reading**

- **Guide:** [YAML Syntax for Beginners](https://yaml.org/spec/1.2/spec.html)
- **Tool:** [YAML Lint (Online Validator)](http://www.yamllint.com/)

## 📝 **Mini Task (Production)**

1. Look at this malformed YAML:
   ```yaml
   services:
   stack:
   image: alpine
   ports:
   - 80:80
   restart:always
   ```
2. Rewrite it correctly using 2-space indentation and fixing the missing spaces.
3. Add a comment explaining what the `image` field does.
4. Predict what error a validator would give for the line `restart:always`.
