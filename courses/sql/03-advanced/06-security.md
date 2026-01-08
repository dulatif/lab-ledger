# 6. Security and Permissions

## 🎯 Learning Goal

Who can do what.

## 🧠 Concept

**GRANT**: Give permission.
`GRANT SELECT ON users TO analyst_role;`

**REVOKE**: Take permission.
`REVOKE DELETE ON users FROM intern_role;`

## 💻 Implementation

Principle of Least Privilege.
App should not connect as `root`. Create an `app_user` with specific rights.

## 🧩 Activity / Challenge

1.  Create a readonly user. Try to Insert.

## 🔑 Key Takeaways

- Security begins at the database connection.
