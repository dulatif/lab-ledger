# Lesson 2: Native Modules (Bridging)

## Learning Goals

- Understand the "Bridge".
- Call Native Code (Swift/Kotlin) from JS.

## 1. The Bridge Concept

React Native runs JS on one thread, Native UI on another. The "Bridge" sends async JSON messages between them.

- JS: "Please vibrate." -> **Bridge** -> Native: "Vibrating!"

## 2. Writing a Native Module (Expo)

Expo makes this easier with **Expo Modules API**. You write Swift/Kotlin code, and Expo generates the JS bindings automatically.

**Swift Example (MyModule.swift)**:

```swift
import ExpoModulesCore

public class MyModule: Module {
  public func definition() -> ModuleDefinition {
    Name("MyModule")

    Function("hello") { (name: String) in
      return "Hello \(name) from Swift!"
    }
  }
}
```

**JS Usage**:

```javascript
import { requireNativeModule } from "expo-modules-core";
const MyModule = requireNativeModule("MyModule");

console.log(MyModule.hello("World")); // "Hello World from Swift!"
```

## 3. JSI (JavaScript Interface)

The "New Architecture" (Fabric/TurboModules) removes the JSON Bridge. C++ is used to let JS call Native functions _synchronously_ and directly.

- **Benefit**: Faster (no serialization).
- **Status**: Default in newer React Native versions.

## Key Takeaways

- Use Native Modules when you need access to a specific platform API not exposed by React Native.
- **Expo Modules** is the modern, easiest way to write native code.
