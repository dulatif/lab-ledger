# Lesson 1: Web Foundations for React Native

## Learning Goals

- Mapping Web Concepts to Native.
- Understanding ES6 features critical for React Native.

## 1. Web vs Native

If you come from a web background, mapping concepts is the fastest way to learn.

| Web Concept      | React Native Equivalent | Note                                           |
| :--------------- | :---------------------- | :--------------------------------------------- |
| `<div>`          | `<View>`                | The basic container.                           |
| `<span>` / `<p>` | `<Text>`                | All text **must** be inside `<Text>`.          |
| `<ul>`, `<li>`   | `<FlatList>`            | Never use map() for long scrolling lists.      |
| `<img>`          | `<Image>`               | Requires source object `{uri: ...}`.           |
| `onClick`        | `onPress`               | No "clicks" on mobile.                         |
| CSS              | `StyleSheet.create`     | Flexbox is the default and only layout engine. |

## 2. Essential JavaScript (ES6+)

You will use these 100 times a day.

### Arrow Functions

```javascript
// Components are functions
const MyComponent = () => { ... }
```

### Destructuring

Extracting props or state.

```javascript
const { name, age } = userObject;
// In function signature
const Profile = ({ name, bio }) => { ... }
```

### Spread Operator (...)

Merging styles or updating state objects immutably.

```javascript
const baseStyle = { padding: 10 };
const activeStyle = { ...baseStyle, backgroundColor: "blue" };
```

## Key Takeaways

- Forget `div` and `className`. Think `View` and `style`.
- Text nodes cannot exist directly inside a `View`; they must be wrapped in `Text`.
- Flexbox is not "optional" like in CSS Grid; it is the **only** way to layout content.
