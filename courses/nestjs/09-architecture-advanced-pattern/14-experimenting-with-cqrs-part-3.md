# 14. Experimenting with CQRS. Part 3

## 🎯 Learning Goal

Implement the **Query Side**. Create a separate model optimized for reading.

## 🧠 Concept

We will create a `GetUsersQuery`.
In a full CQRS setup, this might read from a completely different database (e.g., ElasticSearch or Redis Cache) that is populated by Events.
For now, we'll just simulate a separate read path.

## 💻 Implementation

### 1. The Query

```typescript
export class GetUsersQuery {}
```

### 2. The Handler

```typescript
@QueryHandler(GetUsersQuery)
export class GetUsersHandler implements IQueryHandler<GetUsersQuery> {
  constructor(private repo: UserViewRepository) {} // Notice: UserVIEW Repo

  async execute(query: GetUsersQuery) {
    return this.repo.findAll(); // Fast, no joins, just data
  }
}
```

### 3. Controller

```typescript
@Get()
findAll() {
  return this.queryBus.execute(new GetUsersQuery());
}
```

## 🧩 Activity / Challenge

1.  Implement `GetUsersQuery`.
2.  Create a simple array/mock for the Read Side.
3.  Notice how the Read Side doesn't need entities, validation, or complex logic. It just pumps data out.

## 🔑 Key Takeaways

- **Read Models** (Views) are cheap. You can have `UserListView`, `UserProfileView`, `UserAdminView`, all updated by the same events.
- Optimized for specific UI views.
