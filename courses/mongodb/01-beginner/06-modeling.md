# Lesson 6: Embedded vs References

## Learning Goals

- Data Modeling.
- Normalization vs Denormalization.

## 1. Embedding (Denormalization)

Storing data inside the document.

- **Example**: User has an Address `{ street: "Main St", city: "NY" }` inside the User doc.
- **Pros**: Single read to get everything. Fast.
- **Cons**: Duplication. Hard to update if data changes often.
- **Use When**: "Contains" relationship (Blog Post includes Comments). 1-to-Few.

## 2. Referencing (Normalization)

Storing the `_id` of related data.

- **Example**: `Order` has `customerId`.
- **Pros**: No duplication. Smaller documents.
- **Cons**: Requires `$lookup` (Join) or multiple queries. Slow.
- **Use When**: Many-to-Many. Large datasets. 1-to-Many (>1000).

## Key Takeaways

- Default to Embedding.
- Reference when arrays grow unboundedly.
