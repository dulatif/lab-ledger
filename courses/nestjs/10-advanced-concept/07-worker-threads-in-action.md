# 7. Worker Threads in Action

## 🎯 Learning Goal

Handle CPU-intensive tasks without blocking the main Event Loop.

## 🧠 Concept

Node.js is **Single Threaded**.
If you calculate Fibonacci(50) in a controller, **ALL** other requests to the server hang until it finishes. 🛑

**Worker Threads** allow you to spin up a separate thread to do the heavy lifting, while the main thread keeps serving requests.

## 💻 Implementation

### 1. The Worker Script

```typescript
// src/fib.worker.ts
const { parentPort, workerData } = require("worker_threads");

function fib(n) {
  if (n < 2) return n;
  return fib(n - 1) + fib(n - 2);
}

const result = fib(workerData.value);
parentPort.postMessage(result);
```

### 2. The Service

```typescript
import { Worker } from "worker_threads";

@Injectable()
export class FibService {
  async compute(n: number) {
    return new Promise((resolve, reject) => {
      const worker = new Worker("./dist/fib.worker.js", {
        workerData: { value: n },
      });
      worker.on("message", resolve);
      worker.on("error", reject);
    });
  }
}
```

## 🧩 Activity / Challenge

1.  Create a "blocking" info point without workers. Hit it with 2 tabs. Notice the second tab waits.
2.  Refactor to use a Worker Thread.
3.  Hit it with 2 tabs. Notice they run in parallel! 🚀

## 🔑 Key Takeaways

- **Caveat**: Workers are heavy to start. Use a **Worker Pool** (like `piscina`) for production.
- **Compilation**: NestJS build process (webpack/tsc) needs to know about the worker file to compile it to `dist/`.
