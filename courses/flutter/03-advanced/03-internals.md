# Lesson 3: Flutter Internals

## Learning Goals

- The 3 Trees.
- Widget vs Element vs RenderObject.

## 1. The Widget Tree

Configuration. Cheap. Rebuilt constantly.
`Container(color: red)`

## 2. The Element Tree

The "Glue". Manages lifecycle and state.
Matches old configuration (Widget) to new configuration.

## 3. The Render Tree

The "Painter". Expensive. Calculates layout and paints pixels.
Flutter tries hard _not_ to touch this tree.

## 4. Why it matters?

If you put a Key on a Widget, you tell the Element Tree: "I am the same element, just moved".
This preserves State.

## Key Takeaways

- Widgets are throw-away blueprints.
- Elements are persistent instances.
- RenderObjects do the heavy lifting.
