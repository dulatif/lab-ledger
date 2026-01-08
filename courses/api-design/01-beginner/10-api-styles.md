# 10. Overview of API Styles

## 🎯 Learning Goal

Different ways to design APIs.

## 🧠 Concept

1.  **REST** (Representational State Transfer): Resource-based (`/users/1`). JSON.
2.  **GraphQL**: Query-based. Ask for exactly what you want.
3.  **RPC** (Remote Procedure Call): Action-based (`getUser(1)`). gRPC.
4.  **SOAP**: XML-based. Strict contract.

## 💻 Implementation

- REST is the default for public web APIs.
- gRPC is for internal microservices.
- GraphQL is for complex frontends.

## 🧩 Activity / Challenge

1.  Compare retrieving a user + their posts.
    - REST: 2 requests or 1 custom endpoint.
    - GraphQL: 1 query nested.

## 🔑 Key Takeaways

- No one size fits all.
