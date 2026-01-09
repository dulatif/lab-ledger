# Lesson 9: SectionList & Gestures

## Learning Goals

- Group data with `SectionList`.
- Implement Pull-to-Refresh.

## 1. SectionList

Like `FlatList`, but supports headers. Useful for "Contacts by Letter" or "Settings Groups".

```jsx
const DATA = [
  {
    title: "Main Dishes",
    data: ["Pizza", "Burger", "Risotto"],
  },
  {
    title: "Sides",
    data: ["Fries", "Onion Rings"],
  },
];

<SectionList
  sections={DATA}
  keyExtractor={(item, index) => item + index}
  renderItem={({ item }) => <Text style={styles.item}>{item}</Text>}
  renderSectionHeader={({ section: { title } }) => (
    <Text style={styles.header}>{title}</Text>
  )}
/>;
```

## 2. Pull to Refresh

Add `RefreshControl` to any ScrollView/FlatList.

```jsx
import { RefreshControl } from "react-native";

const [refreshing, setRefreshing] = useState(false);

const onRefresh = React.useCallback(() => {
  setRefreshing(true);
  // Simulate fetching new data
  setTimeout(() => setRefreshing(false), 2000);
}, []);

<FlatList
  refreshControl={
    <RefreshControl refreshing={refreshing} onRefresh={onRefresh} />
  }
  // ...
/>;
```

## Key Takeaways

- `SectionList` expects data in `{ title, data }` format.
- `RefreshControl` is a standard native pattern.
