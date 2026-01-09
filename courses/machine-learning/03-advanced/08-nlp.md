# Lesson 8: NLP Pipeline

## Learning Goals

- Tokenization.
- Embeddings.

## 1. Tokenization

Splitting text into chunks.

- **Word-level**: "Hello", "World". (Huge vocabulary).
- **Subword (BPE/WordPiece)**: "Hel", "lo". (Balance).

## 2. Embeddings

Converting tokens into dense vectors.
Words with similar meanings map to nearby points in vector space.
`King - Man + Woman = Queen`.

## Key Takeaways

- Text is just a sequence of IDs to a model.
- Embeddings capture semantic meaning.
