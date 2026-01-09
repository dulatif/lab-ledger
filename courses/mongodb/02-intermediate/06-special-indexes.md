# Lesson 6: Special Indexes

## Learning Goals

- Multikey.
- TTL.
- Text.

## 1. Multikey Indexes (Arrays)

Indexing an array field (`tags: ["A", "B"]`).
MongoDB creates an index entry for _each element_.
Can be expensive.

## 2. TTL (Time To Live) Indexes

Automatically delete documents after a certain time.
Useful for Sessions, Logs, OTPs.
`db.sessions.createIndex({ "createdAt": 1 }, { expireAfterSeconds: 3600 })`.

## 3. Text Indexes

Basic Full-Text Search.
`$text` operator.
Supports language stemming (running -> run).

## Key Takeaways

- TTL is a background thread (not precise to the millisecond).
- For advanced Search, use Atlas Search (Lucene), not basic Text Indexes.
