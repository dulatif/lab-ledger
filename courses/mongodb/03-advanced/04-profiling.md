# Lesson 4: Database Profiling

## Learning Goals

- Profiler.
- Slow Queries.

## 1. The Profiler

Captures data about operations.

- Level 0: Off.
- Level 1: Log only slow operations (>100ms).
- Level 2: Log everything (Development only!).

```javascript
db.setProfilingLevel(1, { slowms: 100 });
```

## 2. `system.profile`

The collection where profile data is stored.
You can query it like any other collection.
`db.system.profile.find({ millis: { $gt: 1000 } })`

## Key Takeaways

- Keep Level 1 on in production with a reasonable threshold (e.g., 200ms).
- Atlas provides a visual "Performance Advisor".
