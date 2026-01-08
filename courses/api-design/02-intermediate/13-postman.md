# 13. Testing Tools (Postman)

## 🎯 Learning Goal

Manual testing workflow.

## 🧠 Concept

**Postman/Insomnia**: GUI for HTTP.
Allows saving collections, environment variables (Dev vs Prod), and sharing with team.

## 💻 Implementation

1.  Create Collection.
2.  Set `{{baseUrl}}` variable.
3.  Script tests in Javascript tab: `pm.expect(response.code).to.eql(200)`.

## 🧩 Activity / Challenge

1.  Create a request that extracts a Login Token and saves it to a variable for the next request.

## 🔑 Key Takeaways

- Automate your manual tests with scripts.
