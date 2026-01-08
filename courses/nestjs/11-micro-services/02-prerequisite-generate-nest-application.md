# 2. Prerequisite: Generate Nest application

## 🎯 Learning Goal

Setup a NestJS Monorepo.

## 🧠 Concept

Standard `nest new` creates a standard structure.
`nest new` with monorepo mode creates a workspace.

## 💻 Implementation

### 1. Create Workspace

```bash
nest new my-microservices-app
# Choose npm/yarn/pnpm
```

### 2. Convert to Monorepo

By default, it creates an app in `src`. Let's assume we want to restructure.
The CLI automatically handles `apps/` folder if you generate another app.

```bash
nest generate app orders
nest generate app notifications
```

Now you have:

```
apps/
  my-microservices-app/ (the API Gateway usually)
  orders/
  notifications/
libs/
```

## 🧩 Activity / Challenge

1.  Run the commands.
2.  Observed the `nest-cli.json`. It now lists multiple projects.
3.  Run the orders app: `nest start orders`.

## 🔑 Key Takeaways

- **Monorepo**: Shared code, single version of dependencies, easy refactoring.
