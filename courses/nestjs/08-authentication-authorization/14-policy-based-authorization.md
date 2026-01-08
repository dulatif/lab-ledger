# 14. Policy-based Authorization

## 🎯 Learning Goal

Handle complex authorization logic that depends on **Business Rules** and **Data Context**.

## 🧠 Concept

What if the rule is: "Users can strictly edit **their own** posts"?

- RBAC says: "You contain the User Role." -> Pass.
- Claims says: "You have can:edit." -> Pass.
- **Reality**: Pass... but ONLY if `post.authorId === user.id`. This requires checking the **actual data**.

We use **Policies** (often with a library like CASL) to define these rules.

## 💻 Implementation

### 1. The Problem with Guards

Guards don't usually know about the specific _entity_ being accessed (e.g., which Post ID 5 looks like).
We often handle this inside the **Service** layer or a specific **Interceptor**.

### 2. Service-level Check (Simplest)

```typescript
// posts.service.ts
async update(id: number, user: User, updatePostDto: UpdatePostDto) {
  const post = await this.postsRepository.findOne(id);

  if (post.authorId !== user.id) {
    throw new ForbiddenException('You can only edit your own posts');
  }

  // proceed...
}
```

### 3. CASL (Advanced)

CASL allows defining abilities:

```typescript
const { can, cannot, build } = new AbilityBuilder(createMongoAbility);
can("update", "Post", { authorId: user.id });
```

NestJS integrates well with CASL for scalable complex apps.

## 🧩 Activity / Challenge

1.  Implement the simple "Owner Check" in your service method.
2.  Try to update someone else's resource.
3.  Observe the 403 Forbidden error.

## 🔑 Key Takeaways

- **Context Aware**: Real-world auth often depends on _data_, not just _tokens_.
- **Layering**: It's okay to do basic checks (Role) in Guards, and complex checks (Ownership) in Services.
