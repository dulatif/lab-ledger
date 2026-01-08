# 2. Prerequisite: Generate Nest application

## 🎯 Learning Goal

Setup a clean playground for our architectural experiments.

## 🧠 Concept

Since we are going to be moving files around and changing folder structures drastically, it's best to start fresh.
We don't want to break your existing "Cats" application.

## 💻 Implementation

### 1. Generate New Project

```bash
nest new architecture-playground
cd architecture-playground
```

### 2. Install Dependencies

We will use some libraries later for CQRS and DDD.

```bash
npm install @nestjs/cqrs uuid
npm install --save-dev @types/uuid
```

## 🧩 Activity / Challenge

1.  Create the new project.
2.  Verify it runs (`npm run start:dev`).
3.  Open it in your IDE.

## 🔑 Key Takeaways

- A clean slate helps when learning structural patterns.
