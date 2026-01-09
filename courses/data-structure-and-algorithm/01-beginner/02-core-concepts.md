# Lesson 2: Core Concepts (Classes & References)

## Learning Goals

- Implement a Python Class.
- Understand `self`.
- Understand Variables as References (Pointers).

## 1. Classes and Objects

DSA is built on custom objects (Nodes, Edges).

```python
class Dog:
    # Constructor (Initializer)
    def __init__(self, name: str):
        self.name = name  # Instance Variable

    def bark(self):
        print(f"{self.name} says Woof!")

my_dog = Dog("Buddy")
my_dog.bark()
```

### The `__repr__` Method

For debugging, always define `__repr__` so your objects print nicely.

```python
    def __repr__(self):
        return f"Dog(name={self.name})"
```

## 2. Variables are References

In Python, variables are "nametags" that point to objects in memory. This is critical for Linked Lists and Trees.

```python
list_a = [1, 2, 3]
list_b = list_a   # list_b points to the SAME object as list_a

list_b.append(4)

print(list_a) # [1, 2, 3, 4] -> list_a also changed!
```

**Implication for DSA**:
When you pass a list or a Node to a function, you are passing a reference. If the function modifies it, the original object changes.

**How to Copy**:
If you need a separate copy, you must explicitly copy.

```python
list_c = list_a.copy() # Shallow copy
```

## 3. Truthiness

Python simplifies checks:

- Empty list `[]` is `False`.
- `0` and `None` are `False`.
- Everything else is `True`.

```python
stack = []
if not stack:
    print("Stack is empty")
```

## Key Takeaways

- **Classes** bundle data and methods. `__init__` sets up the object.
- **Assignment (`=`)** copies the _reference_, not the data.
- Changing a mutable object (List, Dictionary, Class Instance) inside a function changes it everywhere.
