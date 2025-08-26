# 🔄 React (Web) → React Native Migration Guide

React Native allows you to build **true native mobile apps** using a
React-like syntax.\
Migrating a React web app to React Native is not a direct "convert" but
a **migration** where you:

-   ✅ Reuse **business logic** (state, API calls, utils).\
-   ❌ Rewrite the **UI layer** with native components.\
-   🔄 Replace **web APIs** with React Native or Expo alternatives.

This guide provides a **step-by-step approach**.

------------------------------------------------------------------------

## 1️⃣ Key Differences Between React and React Native

  ------------------------------------------------------------------------
  Feature              React (Web)                React Native (Mobile)
  -------------------- -------------------------- ------------------------
  UI Components        HTML (`div`, `span`)       Native (`View`, `Text`)

  Styling              CSS, Tailwind, Styled      `StyleSheet` or
                                                  libraries (Styled
                                                  Components, NativeWind)

  Navigation           React Router               React Navigation

  DOM Access           `document`, `window`       ❌ Not available

  Storage              localStorage, IndexedDB    AsyncStorage,
                                                  SecureStore

  Animations           CSS animations             Animated API, Reanimated

  File Handling        `<input type="file">`      Expo ImagePicker,
                                                  DocumentPicker

  Push Notifications   Browser Push API           Firebase/APNs, Expo
                                                  Notifications
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 2️⃣ What You Can Reuse

-   **Business logic:** form validation, API clients, data
    transformations.\
-   **State management:** Redux, Zustand, Recoil, Context API.\
-   **Hooks:** custom hooks (except DOM-dependent ones).\
-   **Utility functions:** formatting dates, math operations, etc.

------------------------------------------------------------------------

## 3️⃣ Project Setup

### Option A: Using Expo (Recommended for most cases)

``` bash
npx create-expo-app my-native-app
cd my-native-app
npm start
```

-   Run on Android/iOS via **Expo Go app**.\
-   Provides built-in APIs (camera, push, file system).

### Option B: Using React Native CLI

``` bash
npx react-native init myNativeApp
cd myNativeApp
npx react-native run-android
npx react-native run-ios
```

-   Needed if you require custom native modules.

------------------------------------------------------------------------

## 4️⃣ Component Mapping (Web → Native)

  -----------------------------------------------------------------------
  React Web Component              React Native Equivalent
  -------------------------------- --------------------------------------
  `<div>`                          `<View>`

  `<span>`, `<p>`, `<h1>`          `<Text>`

  `<button>`                       `<Button>` / `<TouchableOpacity>`

  `<input>`                        `<TextInput>`

  `<img>`                          `<Image>`

  `<a>`                            `Linking` API / React Navigation
                                   `Link`

  `<form>`                         Manual state handling / Formik / React
                                   Hook Form

  CSS classes                      `StyleSheet.create()` /
                                   styled-components/native / NativeWind
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 5️⃣ Example Conversion

**React (Web)**

``` jsx
<div className="container">
  <h1>Hello {user.name}</h1>
  <button onClick={logout}>Logout</button>
</div>
```

**React Native**

``` jsx
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';

export default function Home({ user, logout }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello {user.name}</Text>
      <TouchableOpacity onPress={logout} style={styles.button}>
        <Text style={styles.buttonText}>Logout</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  title: { fontSize: 24, fontWeight: 'bold' },
  button: { backgroundColor: '#007bff', padding: 10, borderRadius: 5 },
  buttonText: { color: '#fff' }
});
```

------------------------------------------------------------------------

## 6️⃣ Navigation Migration

-   **React Web:** React Router\
-   **React Native:** React Navigation

Install React Navigation:

``` bash
npm install @react-navigation/native @react-navigation/native-stack
```

Usage:

``` jsx
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={Home} />
        <Stack.Screen name="Profile" component={Profile} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

------------------------------------------------------------------------

## 7️⃣ Storage Migration

-   **Web:** `localStorage`, `sessionStorage`, IndexedDB.\
-   **React Native:**\

``` bash
npm install @react-native-async-storage/async-storage
```

Example:

``` jsx
import AsyncStorage from '@react-native-async-storage/async-storage';

await AsyncStorage.setItem('token', 'abc123');
const token = await AsyncStorage.getItem('token');
```

------------------------------------------------------------------------

## 8️⃣ Handling Web-Specific APIs

  Web API                   React Native Alternative
  ------------------------- ------------------------------------------
  `window.alert()`          `Alert` component
  `localStorage`            AsyncStorage
  `navigator.geolocation`   Expo Location / React Native Geolocation
  File upload (`<input>`)   Expo ImagePicker, DocumentPicker
  Push Notifications        Expo Notifications / Firebase SDK
  WebSockets                Works (same `WebSocket` API)
  Fetch / Axios             Works (no change)

------------------------------------------------------------------------

## 9️⃣ Best Practices for Migration

1.  **Separate business logic from UI**
    -   Create a `/core` folder for logic (API, state, hooks).\
    -   Create a `/components` folder for platform-specific UI.
2.  **Migrate screen by screen**
    -   Start with authentication, then dashboard, then detail screens.
3.  **Test early on devices**
    -   Some things work in the simulator but fail on real devices
        (e.g., camera, push).
4.  **Use Expo for speed unless you need heavy native code**.

------------------------------------------------------------------------

## 🔟 Migration Checklist

-   [ ] Setup new Expo/React Native project.\
-   [ ] Copy business logic (`/services`, `/utils`, `/hooks`).\
-   [ ] Install state management (if used in web).\
-   [ ] Replace UI components with RN equivalents.\
-   [ ] Replace routing (React Router → React Navigation).\
-   [ ] Replace storage (localStorage → AsyncStorage).\
-   [ ] Replace forms (`<input>` → `TextInput` + Formik/React Hook
    Form).\
-   [ ] Test API calls (same axios/fetch).\
-   [ ] Add native modules (camera, push, geolocation as needed).\
-   [ ] Optimize performance (flat lists, avoid heavy DOM-like nesting).

------------------------------------------------------------------------

## 📌 Summary

-   ✅ Business logic is reusable.\
-   ❌ UI must be **rewritten** using React Native components.\
-   🔄 Replace web APIs with **AsyncStorage, React Navigation, Expo
    APIs**.\
-   🚀 Use **Expo** for faster migration unless advanced native code is
    needed.
