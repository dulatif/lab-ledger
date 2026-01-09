# Lesson 2: Data Transformation

## Learning Goals

- `$project`.
- `$addFields`.
- `$unwind`.

## 1. `$project`

Reshapes the document 1-to-1.
Select fields, rename fields, or calculate new fields.
Can remove the `_id`.

## 2. `$addFields` (or `$set`)

Adds new fields without removing existing ones.
Useful for calculating totals or concatenating strings.

## 3. `$unwind`

Deconstructs an array field from the input documents to output a document for _each_ element.
`{ id: 1, tags: ["A", "B"] }` ->
`{ id: 1, tags: "A" }`, `{ id: 1, tags: "B" }`.
Crucial before grouping by array elements.

## Key Takeaways

- Use `$project` last to format the final output.
- `$unwind` increases the number of documents (can explode memory usage if not careful).
