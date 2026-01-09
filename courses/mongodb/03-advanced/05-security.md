# Lesson 5: Security Best Practices

## Learning Goals

- Authentication.
- Authorization (RBAC).
- Network Access.

## 1. Authentication

Verify identity.

- **SCRAM**: Username/Password.
- **X.509**: Certificate-based (Machine-to-Machine).
- **LDAP**: Enterprise integration.

## 2. Authorization (RBAC)

Role-Based Access Control.

- `read`: Read-only.
- `readWrite`: Read and Write.
- `dbAdmin`: Index management.
- `userAdmin`: User management.
- **Principle of Least Privilege**: Give only what is needed.

## 3. Network Access

- **IP Whitelisting**: Only allow trusted IPs (App Servers, VPN).
- **TLS/SSL**: Encrypt data in transit (Default in Atlas).
- **Encryption at Rest**: Encrypt data on disk.

## Key Takeaways

- Never leave a MongoDB instance open to the public internet (0.0.0.0/0) without auth.
- Enable Access Control before deploying.
