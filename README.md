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

## Basic Code Example

Here's an example `App.js` file (`App.tsx` if your project was set up with Typescript):

```
import React, { useMemo, useRef } from 'react';
import {Button, View, StyleSheet, Text, SafeAreaView} from 'react-native';

import { GestureHandlerRootView } from 'react-native-gesture-handler';
import BottomSheet, {BottomSheetSectionList} from '@gorhom/bottom-sheet';

export default function App() {
  // Tracks the state of the bottom drawer
  const bottomSheetRef = useRef(null);
  // The positions that the bottom drawer will snap to
  const snapPoints = useMemo(() => ['25%', '100%'], []);

  return (
    <SafeAreaView style={styles.safeContainer}>
      <GestureHandlerRootView style={styles.container}>
        <Button
            title="Open Bottom Sheet"
            onPress={() => {bottomSheetRef.current.snapToIndex(0);}}
            />
        <BottomSheet
              ref={bottomSheetRef}
              index={0}
              snapPoints={snapPoints}
              enablePanDownToClose={true}
              >
          <View>
            <Text>Please hire me.</Text>
          </View>
        </BottomSheet>
      </GestureHandlerRootView>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#def',
    alignItems: 'center',
    justifyContent: 'center',
  },
  safeContainer: {
    flex: 1,
  }
});
```

Let's step through the `App()` function.

The bottom sheet will need to track its state--whether it's visible on screen, and how far up the screen it currently is. We create a ref to track this state:

```
const bottomSheetRef = useRef(null);
```

The bottom sheet can snap itself to several locations on screen. We configure these snap points by creating an array of descriptions:

```
const snapPoints = useMemo(() => ['25%', '100%'], []);
```

In order for the bottom sheet to work correctly, the root view of the application must be wrapped in a `<GestureHandlerRootView>` tag. This is responsible for capturing the swipe gestures that control the bottom sheet's position on screen.

We create a `Button` that pops up the bottom sheet:

```
<Button
    title="Open Bottom Sheet"
    onPress={() => {bottomSheetRef.current.snapToIndex(0);}}
    />
```

The call `snapToIndex(0)` moves the bottom sheet to one of the snap points specified earlier. In this case, we specify index 0, which earlier was defined to 25% of the screen.

Finally, we specify the bottom sheet itself:

```
<BottomSheet
      ref={bottomSheetRef}
      index={0}
      snapPoints={snapPoints}
      enablePanDownToClose={true}
      >
```

The `ref` and `snapPoints` parameters connect the bottom sheet to the `bottomSheetRef` and `snapPoints` variables we defined earlier. The parameter named `index` specifies which snap point the bottom sheet should be at when it first appears. Here we chose 0, which makes it visible at the first snap point we defined earlier. If we wanted it to start out hidden, we could have specified -1. Finally, `enablePanDownToClose` specifies whether the user can dismiss the bottom sheet by flinging it down.

