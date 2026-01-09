# Lesson 3: String Structures (Tries)

## Learning Goals

- Implement a Prefix Tree (Trie).
- Optimize Autocomplete.

## 1. What is a Trie?

A tree optimized for Strings.

- Root is empty.
- Edges are characters (`'a'`, `'b'`...).
- Nodes mark "End of Word".

**Use Case**:

- Autocomplete ("Type 'app' -> suggest 'apple', 'app store'").
- Spell Checker.

## 2. Implementation

Dictionary of Dictionaries!

```python
class TrieNode:
    def __init__(self):
        self.children = {} # Map char -> TrieNode
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end
```

## 3. Complexity

- **Time**: `O(L)` where L is word length. Independent of how many words are in the DB!
- **Space**: High (stores every character).

## Key Takeaways

- Tries are faster than Hash Tables for Prefix searches.
- **Prefix Search**: `O(L)` vs Hash Map `O(N * L)` (checking every key).
