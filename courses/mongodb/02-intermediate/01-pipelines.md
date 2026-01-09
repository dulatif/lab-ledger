# Lesson 1: Introduction to Pipelines

## Learning Goals

- Aggregation Pipeline.
- `$match`, `$group`, `$sort`.

## 1. The Pipeline Concept

Data flows through a series of "Stages".
`Collection -> Stage 1 -> Transformation -> Stage 2 -> Result`.
Similar to UNIX pipes: `grep | sort | uniq`.

## 2. Basic Stages

- `$match`: Filter documents (Like `WHERE`). **Always put this first** to reduce data processing.
- `$sort`: Order documents (`1` asc, `-1` desc).
- `$limit`: Take top N results.

## 3. The `$group` Stage

Aggregating data (Like `GROUP BY`).
Requires an `_id` to group by (set to `null` for total sum).

```javascript
db.sales.aggregate([
  { $match: { status: "A" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } },
]);
```

## Key Takeaways

- The Aggregation Framework is Turing Complete. You can do almost anything.
- Order of stages matters immensely for performance.
