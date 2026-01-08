# 10. Chapter 13 - Review Quiz

## ❓ Questions

### 1. Which decorator injects a Mongoose Model?

- [ ] `@InjectRepository()`
- [ ] `@InjectModel()`
- [ ] `@InjectConnection()`

### 2. What is required for MongoDB Transactions?

- [ ] A paid license
- [ ] A Replica Set
- [ ] Sharding enabled

### 3. How do you define a property in a Schema?

- [ ] `@Column()`
- [ ] `@Field()`
- [ ] `@Prop()`

## 💡 Answers

### 1. Which decorator injects a Mongoose Model?

> **`@InjectModel()`** > `@InjectRepository` is for TypeORM.

### 2. What is required for MongoDB Transactions?

> **A Replica Set**
> Single node instances (historically) did not support multi-document transactions.

### 3. How do you define a property in a Schema?

> **`@Prop()`**
> It automates the creation of the Mongoose schema definition object.
