# Lesson 6: Media & Controls

## Learning Goals

- Display Images (Local & Remote).
- Use Modals and Switches.

## 1. Image

Displaying images requires defining the size (width/height), otherwise they might not show up!

**Remote URL**:

```jsx
<Image
  source={{ uri: "https://placekitten.com/200/200" }}
  style={{ width: 200, height: 200 }}
/>
```

**Local Asset**:

```jsx
<Image source={require("./assets/logo.png")} />
```

- **Local Note**: React Native automatically calculates dimensions for local assets, so explicit style is optional (but recommended).

## 2. Modal

An overlay that covers the entire screen.

```jsx
const [modalVisible, setModalVisible] = useState(false);

<Modal
  animationType="slide"
  transparent={true}
  visible={modalVisible}
  onRequestClose={() => setModalVisible(false)} // Android Back button
>
  <View style={styles.centeredView}>
    <View style={styles.modalView}>
      <Text>Hello!</Text>
      <Button title="Close" onPress={() => setModalVisible(false)} />
    </View>
  </View>
</Modal>;
```

## 3. Switch (Toggle)

The native toggle button.

```jsx
<Switch
  trackColor={{ false: "#767577", true: "#81b0ff" }}
  thumbColor={isEnabled ? "#f5dd4b" : "#f4f3f4"}
  onValueChange={toggleSwitch}
  value={isEnabled}
/>
```

## Key Takeaways

- **Images**: Always define `width` and `height` for network images.
- **ResizeMode**: Use `contain` vs `cover` to control how the image scales (like CSS `object-fit`).
- **Modal**: Requires careful styling to center content (it fills the screen by default).
