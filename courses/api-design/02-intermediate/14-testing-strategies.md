# 14. Testing Strategies

## 🎯 Learning Goal

Pyramid of Testing.

## 🧠 Concept

1.  **Unit**: Test individual function (e.g., Input Validator). Fast.
2.  **Integration**: Test API + DB. (Mock external services).
3.  **Functional/E2E**: Real user flow. Slow but realistic.

## 💻 Implementation

Focus heavily on Integration tests for APIs. Mock the payment gateway, but use a real (Dockerized) Test Database.

## 🧩 Activity / Challenge

1.  Why is mocking the DB often bad for API tests? (You miss SQL errors).

## 🔑 Key Takeaways

- Integration tests provide the best ROI for Backend APIs.
