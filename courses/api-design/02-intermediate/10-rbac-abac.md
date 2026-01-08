# 10. Access Control (RBAC vs ABAC)

## 🎯 Learning Goal

Fine-grained permission models.

## 🧠 Concept

**RBAC (Role-Based)**:
User has Role `Admin`. `Admin` has Permission `Delete User`.
Simple. Easy to audit.

**ABAC (Attribute-Based)**:
"Allow if User.Department == Resource.Department AND Time < 5PM".
Dynamic and complex.

## 💻 Implementation

Start with RBAC. Move to ABAC only if necessary.

## 🧩 Activity / Challenge

1.  Scenario: "User can only edit their own posts".
    - RBAC: Needs 'EditPost' permission? Too broad.
    - Relationship-Based (ReBAC): Check ownership (Owner).

## 🔑 Key Takeaways

- RBAC covers 80% of use cases.
