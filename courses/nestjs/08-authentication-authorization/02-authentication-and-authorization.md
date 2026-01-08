# 2. Authentication and Authorization

## 🎯 Learning Goal

Clearly distinguish between **AuthN** (Authentication) and **AuthZ** (Authorization).

## 🧠 Concept

People often confuse these two, but they are distinct steps in the security flow.

### 🕵️ Authentication (AuthN)

- **Question**: "Who are you?"
- **Action**: Verifying credentials (username/password, API Key, fingerprint).
- **Outcome**: We know the user's **Identity**.
- **Analogy**: Showing your ID card at the airport security check.

### 👮 Authorization (AuthZ)

- **Question**: "What are you allowed to do?"
- **Action**: Checking permissions against the authenticated identity.
- **Outcome**: Access Granted or Denied.
- **Analogy**: Checking if your boarding pass allows you into the First Class Lounge.

## 💻 Implementation

In NestJS:

- **AuthN** is usually handled by **Passport Strategies** (Local, JWT, OAuth).
- **AuthZ** is usually handled by **Guards** (`@Roles()`, `PoliciesGuard`).

## 🧩 Activity / Challenge

Classify the following scenarios as AuthN or AuthZ:

1.  User types in email and password.
2.  System checks if user is an 'admin'.
3.  User scans their face to unlock phone.
4.  System checks if user owns the post they are trying to edit.

## 🔑 Key Takeaways

- **AuthN** = Identification.
- **AuthZ** = Permissions.
- AuthN always comes _before_ AuthZ.
