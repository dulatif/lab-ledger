# Lesson 4: React Navigation (Stack)

## Learning Goals

- Setup React Navigation.
- Create a Stack Navigator.
- Navigate between screens.

## 1. Installation

React Navigation is the standard library. It requires `react-native-screens` and `react-native-safe-area-context`.

```bash
npm install @react-navigation/native @react-navigation/native-stack
```

## 2. Creating a Stack

A "Stack" puts screens on top of each other (like a deck of cards).

```jsx
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";

const Stack = createNativeStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};
```

## 3. Moving Between Screens

Every screen component gets a `navigation` prop.

```jsx
const HomeScreen = ({ navigation }) => {
  return (
    <Button
      title="Go to Details"
      onPress={() => navigation.navigate("Details")}
    />
  );
};
```

- `navigation.push('Details')`: Adds another instance of Details (allows multiples).
- `navigation.goBack()`: Simulates back button.

## Key Takeaways

- **NavigationContainer** must wrap the whole app.
- **Stack** is the most common navigation pattern.
