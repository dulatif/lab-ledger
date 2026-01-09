# Lesson 3: Push Notifications

## Learning Goals

- Local vs Remote Notifications.
- Setting up Expo Notifications.

## 1. Local Notifications

Scheduled by the app itself (e.g., "Alarm Clock", "Reminder").

```jsx
import * as Notifications from "expo-notifications";

const verifyPermission = async () => {
  const { status } = await Notifications.requestPermissionsAsync();
  if (status !== "granted") {
    alert("Permission needed!");
    return false;
  }
  return true;
};

const sendLocal = async () => {
  await Notifications.scheduleNotificationAsync({
    content: { title: "Wake up!", body: "It's morning." },
    trigger: { seconds: 5 },
  });
};
```

## 2. Remote Notifications

Sent from your backend server.

1.  **Get Token**: App asks Expo/FCM for a unique token (e.g., `ExponentPushToken[...]`).
2.  **Send to Backend**: App sends this token to your database (`UPDATE users SET push_token = ?`).
3.  **Trigger**: Backend sends a POST request to Expo Push API -> Apple/Google -> User's Phone.

## 3. Handling Taps

When a user taps a notification, you usually want to navigate them to a specific screen.

```jsx
const responseListener = useRef();

useEffect(() => {
  responseListener.current =
    Notifications.addNotificationResponseReceivedListener((response) => {
      const screen = response.notification.request.content.data.screen;
      navigation.navigate(screen);
    });
}, []);
```

## Key Takeaways

- **Permissions**: Mandatory on iOS and Android 13+.
- **Token Rotation**: Tokens change if the user uninstalls. Handle errors on your backend.
