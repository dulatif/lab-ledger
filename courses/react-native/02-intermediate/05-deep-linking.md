# Lesson 5: Params & Deep Linking

## Learning Goals

- Pass data between screens.
- Configure Deep Linking (URL -> App).

## 1. Passing Parameters

Pass an object as the second argument to `.navigate()`.

```jsx
// Sender
navigation.navigate("Details", { itemId: 86, otherParam: "test" });

// Receiver
const DetailsScreen = ({ route, navigation }) => {
  const { itemId, otherParam } = route.params;

  return <Text>Item: {itemId}</Text>;
};
```

## 2. Deep Linking

Allow users to open your app via a URL like `myapp://details/86`.

**Config Object**:

```javascript
const linking = {
  prefixes: ["myapp://", "https://myapp.com"],
  config: {
    screens: {
      Home: "home",
      Details: "details/:itemId", // Dynamic param
    },
  },
};

<NavigationContainer linking={linking}>...</NavigationContainer>;
```

## Key Takeaways

- **Route Prop**: Use `route.params` to read data.
- **Deep Linking**: Crucial for sharing content or handling email verification links.
