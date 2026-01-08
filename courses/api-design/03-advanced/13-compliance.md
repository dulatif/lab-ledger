# 13. Legal Compliance (GDPR, PCI, HIPAA)

## 🎯 Learning Goal

Not going to jail.

## 🧠 Concept

- **GDPR (Europe)**: User right to be forgotten. API must have "Delete User" that actually wipes data everywhere.
- **PCI DSS (Cards)**: Never log credit card numbers.
- **HIPAA (Health)**: Strict encryption/audit logs for PII.

## 💻 Implementation

Masking in logs.
Encryption at rest.
Audit trails (Who accessed record X?).

## 🧩 Activity / Challenge

1.  Design your logging middleware to automatically mask `password` and `cc_number`.

## 🔑 Key Takeaways

- Compliance requirements drive architectural decisions (e.g., Data Residency).
