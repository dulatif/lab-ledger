# Lesson 5: Query Analysis

## Learning Goals

- `explain()`.
- Winning Plan.

## 1. Using `.explain("executionStats")`

Append to any query to see how it ran.
`db.users.find({...}).explain(...)`

## 2. Key Metrics

- **totalKeysExamined**: How many index entries looked at.
- **totalDocsExamined**: How many full documents read.
- **nReturned**: How many results given.
- **Ideal Ratio**: Keys Examined == nReturned.

## 3. Stages

- **IXSCAN**: Index Scan (Good).
- **COLLSCAN**: Collection Scan (Bad, usually).
- **FETCH**: Retrieving document content from disk/RAM.

## 4. Covered Query

If the index contains ALL the fields in the query AND the projection, MongoDB doesn't need to fetch the document.
`totalDocsExamined: 0`. Super fast.

## Key Takeaways

- If `totalDocsExamined` >> `nReturned`, you need an index or a better filter.
