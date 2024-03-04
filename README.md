# Using a bottom sheet with React Native
## Introduction

Bottom sheets are UI concept in which new information appears "peeking in" from the bottom of the screen. This allows the user to see some of the data without losing their current context. If they want to see the rest, they simply swipe up on the bottom sheet. Otherwise, they can dismiss it by swiping down.

Here's an example of Apple Maps using a bottom sheet to display information about an address:

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

First install the required packages by issuing this command from your project root directory:

```
yarn add @gorhom/bottom-sheet react-native-reanimated react-native-gesture-handler
```

Then follow steps 2 and 3 from the [React Native Reanimated install instructions](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started/#step-2-add-reanimateds-babel-plugin).

Finally, if you are using Xcode, update your CocoaPods:

```
npx pod-install
```

## Basic Code Example

Here's an example `App.js` file (`App.tsx` if your project was set up with Typescript):

```javascript
import React, {useMemo, useRef} from 'react';
import {Button, StyleSheet, Text, SafeAreaView} from 'react-native';

import {GestureHandlerRootView} from 'react-native-gesture-handler';
import BottomSheet from '@gorhom/bottom-sheet';

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
          <Text>Please hire me.</Text>
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

Here's what it looks like in the emulator:

![Screenshot of first example](/SimpleExample.png)

Let's step through the `App()` function.

The bottom sheet will need to track its state: how far up the screen it currently extends, if it's visible at all. We create a ref to track this state:

```javascript
const bottomSheetRef = useRef(null);
```

The bottom sheet can snap itself to several locations on screen. We specify these snap points as an array:

```javascript
const snapPoints = useMemo(() => ['25%', '100%'], []);
```

In order for the bottom sheet to work correctly, the root view of the application must be wrapped in a `<GestureHandlerRootView>` tag. This is responsible for capturing the swipe gestures that control the bottom sheet's position on screen.

We create a `Button` that pops up the bottom sheet:

```javascript
<Button
    title="Open Bottom Sheet"
    onPress={() => {bottomSheetRef.current.snapToIndex(0);}}
    />
```

The call `snapToIndex(0)` moves the bottom sheet to one of the snap points specified earlier. In this case, we specify index 0, which earlier was defined to 25% of the screen. (This and other methods are listed in [the official documentation](https://ui.gorhom.dev/components/bottom-sheet/methods).)

Finally, we specify the bottom sheet itself:

```javascript
<BottomSheet
      ref={bottomSheetRef}
      index={0}
      snapPoints={snapPoints}
      enablePanDownToClose={true}
      >
```

The `ref` and `snapPoints` parameters connect the bottom sheet to the `bottomSheetRef` and `snapPoints` variables we defined earlier. The parameter named `index` specifies which snap point the bottom sheet should be at when it first appears. Here we chose 0, which makes it visible at the first snap point we defined earlier. If we wanted it to start out hidden, we could have specified -1. Finally, `enablePanDownToClose` specifies whether the user can dismiss the bottom sheet by flinging it down.

You can find the [full set of properties in the official documentation](https://ui.gorhom.dev/components/bottom-sheet/props).

## Bottom-Sheet-Ready Views

There is a subtle problem with including scrollable content inside a bottom sheet: How does the system distinguish between the user scrolling through the content inside the bottom sheet versus the user moving the bottom sheet itself? Fortunately, we have a solution: The bottom sheet library comes with a full suite of integrated scrollable components. These include a [ScrollView](https://ui.gorhom.dev/components/bottom-sheet/components/bottomsheetscrollview), [FlatList](https://ui.gorhom.dev/components/bottom-sheet/components/bottomsheetflatlist), [SectionList](https://ui.gorhom.dev/components/bottom-sheet/components/bottomsheetsectionlist), and [VirtualizedList](https://ui.gorhom.dev/components/bottom-sheet/components/bottomsheetvirtualizedlist). (There's also a [TextInput](https://ui.gorhom.dev/components/bottom-sheet/components/bottomsheettextinput)!) All are designed to work as drop-in replacements for the standard React Native components.

Here's an example project that uses a SectionList:

```javascript
import React, {useCallback, useMemo, useRef} from 'react';
import {Button, StyleSheet, Text, SafeAreaView} from 'react-native';

import {GestureHandlerRootView} from 'react-native-gesture-handler';
import BottomSheet, {BottomSheetSectionList} from '@gorhom/bottom-sheet';

export default function App() {
  // Tracks the state of the bottom drawer
  const bottomSheetRef = useRef(null);
  // The positions that the bottom drawer will snap to
  const snapPoints = useMemo(() => ['25%', '100%'], []);

  // Data for the list example
  const sections = useMemo(() => {
    return [
      {title: 'Languages', data: ['C', 'C++', 'Java', 'JavaScript', 'Python']},
      {title: 'Frameworks', data: ['Django', 'React Native']}
    ];
  }, []);

  // Render a section header
  const renderSectionHeader = useCallback(({section}) => (
    <Text style={styles.sectionHeader}>{section.title}</Text>
  ), []);

  // Render a list entry
  const renderItem = useCallback(({item}) => (
    <Text style={styles.entry}>{item}</Text>
  ), []);

  // Returns the lead header component for the list
  const renderListHeader = useCallback(() => (
    <Text style={styles.listHeader}>My Skills</Text>
  ), []);

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
          <BottomSheetSectionList
              sections={sections}
              renderSectionHeader={renderSectionHeader}
              renderItem={renderItem}
              ListHeaderComponent={renderListHeader}
              />
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
  },
  sectionHeader: {
    fontWeight: 'bold',
    fontSize: 20
  },
  entry: {
    fontWeight: 'normal',
    fontSize: 16
  },
  listHeader: {
    fontWeight: 'bold',
    fontSize: 24
  }
});
```

Here's what it looks like in the emulator:

![Screenshot of HeaderList example](/HeaderListExample.png)

Let's go over the changes to `App()`.

First, we define the data that our section list will use:

```javascript
const sections = useMemo(() => {
  return [
    {title: 'Languages', data: ['C', 'C++', 'Java', 'JavaScript', 'Python']},
    {title: 'Frameworks', data: ['Django', 'React Native']}
  ];
}, []);
```

This comes as a list of sections, where each section is an object containing a `title` member and a `data` member. The `data` member must be another list.

We have a callback for rendering a section header:

```javascript
const renderSectionHeader = useCallback(({section}) => (
  <Text style={styles.sectionHeader}>{section.title}</Text>
), []);
```

Since our section title objects are just strings, we simply create a text view containing that string.

We have a callback for rendering an individual entry:

```javascript
const renderItem = useCallback(({item}) => (
  <Text style={styles.entry}>{item}</Text>
), []);
```

Same story: Our entries are just strings, so we wrap them in text views.

I also want to add a special header at the start of the list, so here's a callback for that:

```javascript
const renderListHeader = useCallback(() => (
  <Text style={styles.listHeader}>My Skills</Text>
), []);
```

Now, all we have to do when creating the list is pass it the data and callbacks we already created:

```javascript
<BottomSheetSectionList
    sections={sections}
    renderSectionHeader={renderSectionHeader}
    renderItem={renderItem}
    ListHeaderComponent={renderListHeader}
    />
```
