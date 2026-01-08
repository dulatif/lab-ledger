# 2. Prerequisite: Generate Nest application

## 🎯 Learning Goal

Setup a clean environment for advanced experiments.

## 🧠 Concept

We will create a specific project for testing performance and advanced patterns.

## 💻 Implementation

### 1. Generate Project

```bash
nest new advanced-nest
cd advanced-nest
```

### 2. Install Tools

We might need `autocannon` later for performance testing.

```bash
npm install -g autocannon
```

## 🧩 Activity / Challenge

1.  Generate the project.
2.  Open `src/main.ts`.
3.  Look at `NestFactory.create`. We usually just wait for it to finish. In this chapter, we will interact with the _application instance_ itself more.

## 🔑 Key Takeaways

- Standard setup, but different mindset.
