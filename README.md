# Using a bottom sheet with React Native
## Introduction

Bottom sheets are UI concept in which new information appears "peeking in" from the bottom of the screen. This allows the user to scan the top part of the data without losing their current context. If they want to see the rest, they simply swipe up on the bottom sheet. Otherwise, they can dismiss it.

![An example of a bottom sheet in Apple Maps](/AppleMaps.png)

[Mo Gorham](https://gorhom.dev) has created an excellent [bottom sheet implementation for React Native](https://ui.gorhom.dev/components/bottom-sheet/). We're going to explore how to use it in your app!

## Installation

The install process varies depending on whether you are using Expo Go or building a bare app with React Native CLI.

### Expo Go

The install process for Expo Go is one step! Go to your project root directory, and execute:

```
npx expo install @gorhom/bottom-sheet react-native-reanimated react-native-gesture-handler
```

### React Native CLI

First install the required packages with this command from your project root directory:

```
yarn add @gorhom/bottom-sheet react-native-reanimated react-native-gesture-handler
```

Then follow steps 2 and 3 from the [React Native Reanimated install instructions](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started/#step-2-add-reanimateds-babel-plugin).
