# 16. What's Multi-tenancy?

## 🎯 Learning Goal

Understand the architecture of SaaS applications.

## 🧠 Concept

**Single Tenant**: 1 Customer = 1 Server + 1 DB.
(Expensive to scale).

**Multi Tenant**: 1000 Customers = 1 Server + X DBs.
(Efficient, hard to code).

Strategies:

1.  **Database per Tenant**: Tenant A connects to `db_a`, Tenant B to `db_b`.
2.  **Schema per Tenant**: Postgres Schemas `schema_a`, `schema_b`.
3.  **Row Level Security**: Everyone in `users` table, filtered by `tenant_id` column.

## 💻 Implementation

NestJS shines in Strategy 1 & 2 because of **Durable Providers**.
We can inject a `Connection` that is specific to the Tenant.

## 🧩 Activity / Challenge

1.  Imagine a request header `X-Tenant-Id: acme`.
2.  The app needs to read this header and switch the Database Connection dynamically.

## 🔑 Key Takeaways

- **Isolation**: Critical. Tenant A must NEVER see Tenant B's data.
- **Durable Providers** make this efficient (1000 connections for 1000 tenants, not 1,000,000 connections for 1,000,000 requests).
