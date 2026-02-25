# 💡 ACS03.1: Comments as a Smell

**Outline:**

- **The Smell (SEEI):** Recognizing when a comment is not helpful, but is actually hiding unclear code (a "deodorant" for a smell).
- **The Refactor (PPP):** Applying "Extract Method" and "Introduce Explaining Variable" to make the code self-documenting.
- **Your Refactoring Mission (Production):** Time to eliminate comments by improving the code they describe.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Comments as Deodorant**
The most insidious comments are those that apologize for or explain confusing code. Instead of fixing the underlying problem, we put a comment on it like a deodorant. While well-intentioned, these comments are a liability. The code is the single source of truth; comments can rot and lie, but the code never does.

**The Negative Impact: The Danger of Lies**

- **They Rot:** Code changes, but comments are rarely updated. An out-of-date comment actively misleads developers.
- **They Hide Bad Design:** A comment often serves as an excuse not to refactor (e.g., choosing a better name or extracting logic).
- **They Create Noise:** Files cluttered with obvious comments make it harder to read the actual code.

**Example of the Smell: Explaining a Complex Condition**
The comment here is a red flag, needed only because the `if` statement is unreadable.

```ts
// Check if the user is eligible for a premium discount.
// This is true if they are a logged-in subscriber with more than 100 points,
// or if they are an admin user.
if (
  (user.isLoggedIn() && user.isSubscriber() && user.getPoints() > 100) ||
  user.isAdmin()
) {
  // apply premium discount...
}
```

The comment is the smell. The unreadable condition is the underlying problem.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Make the Code Tell the Story**
Make the code so clear that the comment becomes redundant.

1. **Introduce Explaining Variable:** Extract complex expressions into well-named variables.
2. **Extract Method:** Extract blocks of code into methods whose names describe the task.

**Step-by-Step Implementation: Refactoring the Eligibility Check**
Let's make the code itself explain the logic.

```ts
// Step 1: Extract into explaining variables
const isHighValueSubscriber =
  user.isLoggedIn() && user.isSubscriber() && user.getPoints() > 100;

// The code now reads like the original comment
if (isHighValueSubscriber || user.isAdmin()) {
  // apply premium discount...
}

// OR, using Extract Method:
class User {
  public isEligibleForPremiumDiscount(): boolean {
    const isHighValueSubscriber =
      this.isLoggedIn() && this.isSubscriber() && this.getPoints() > 100;
    return isHighValueSubscriber || this.isAdmin();
  }
}

// Client code is now incredibly clear
if (user.isEligibleForPremiumDiscount()) {
  // apply premium discount...
}
```

The original comment is now unnecessary and should be deleted.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Comment Explains Logic):**
  ```ts
  // Calculate the total area including the margin
  const totalArea = (width + margin * 2) * (height + margin * 2);
  ```
- **After (Self-Documenting):**
  ```ts
  const widthWithMargin = width + margin * 2;
  const heightWithMargin = height + margin * 2;
  const totalArea = widthWithMargin * heightWithMargin;
  ```
- **The Improvement:** The code is the single source of truth. No risk of outdated comments. Logic is broken down into understandable, maintainable steps.

## 🤔 **Reflective Questions**

1. What are some examples of _good_ comments? (e.g., explaining the _why_ behind a workaround).
2. How is a documentation comment (JSDoc/TSDoc) different from a code smell comment?
3. How does trying to write comment-free code improve your overall code quality?

## 📚 **Further Reading**

- **The Bible:** "Refactoring" by Martin Fowler - "Introduce Explaining Variable" and "Extract Method."
- **Clean Code:** Robert C. Martin's "Clean Code" chapter on comments.
- **Refactoring.guru:** [Comments page](https://refactoring.guru/smells/comments).

## 📝 **Mini Task (Production)**

Refactor the payment processing logic below to make it self-documenting and **delete every comment**.

**Your Mission:**

1. Extract logic into variables or methods.
2. Ensure the code flow is explained by the code itself.

```ts
function processPayment(user, cart) {
  // First, calculate the total price of all items in the cart.
  let total = 0;
  for (const item of cart.items) {
    total += item.price;
  }

  // Then, check if the user has a coupon and apply the discount.
  if (user.hasCoupon()) {
    total = total * 0.9; // 10% discount
  }

  // Finally, charge the user's credit card.
  CreditCardService.charge(user.getCard(), total);
}
```
