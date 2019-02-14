# Custom fonts

Adding custom fonts to your react native application will become a requirement sooner or later.
This questions gets asked _a lot_ and this document aims to help you on your way.

## Adding custom fonts

First, let's look at how to add custom fonts.

### Expo

For expo you can use the SDK to load fonts:

```ts
import React, { Component } from 'react';
import { Font } from 'expo';

export default class App extends Component {
  state = {
    fontLoaded: false,
  };

  async componentDidMount() {
    await Font.loadAsync({
      'open-sans-bold': require('./assets/fonts/Your-Font.ttf'),
    });
    
    this.setState({ fontLoaded: true });
  }

  render() {
    if (!this.state.fontLoaded) {
      return null;
    }

    return <YourApp />;
  }
}
```

- `./assets/fonts/Your-Font.ttf` is the font file you added to your project. The directory names can be anything you want, although `./assets/fonts` is recommended.
- It is recommended to load your fonts in your `<App />` _(or whatever your top-level component is)_ to prevent your text from flashing once the font loads.

You can read more [in the expo documentation](https://docs.expo.io/versions/latest/guides/using-custom-fonts/).

### Ejected app

All you have to do is add a key `rnpm` (react native package manager) to your `package.json`.

Doing this will tell react-native-cli to copy the fonts over to the native side the next time you call `react-native link`.

The relevant part for your `package.json` file:

```json
{
  "rnpm": {
    "assets": [
      "./assets/fonts/"
    ]
  },
}
```

This directory (`./assets/fonts/`) is relative to your `package.json` file, and is where you'll store your custom fonts.


## Using custom fonts

Now that you've added and/or loaded your fonts, you can start using them. 

Using custom fonts is simple. In your styles, add the following:

```ts
import { StyleSheet } from 'react-native';

export const styles = StyleSheet.create({
  someKey: {
    fontFamily: 'TheFontHere',
  },
});
```

Where someKey is the key used to style a component.

### Exceptions

Android and iOS have different rules.

On android you can use the name of the font as-is, and use it:

```ts
import { StyleSheet } from 'react-native';

export const styles = StyleSheet.create({
  someKey: {
    fontFamily: 'Comfortaa Bold',
  },
});
```

On iOS however, spaces need to be replaced by dashes:


```ts
import { StyleSheet } from 'react-native';

export const styles = StyleSheet.create({
  someKey: {
    fontFamily: 'Comfortaa-Bold',
  },
});
```

### Pro-tip

An easy way of using a different value for iOS and Android is using `Platform.select()`.

Here's an example:

```ts
import { StyleSheet } from 'react-native';

export const styles = StyleSheet.create({
  someKey: {
    fontFamily: Platform.select({ ios: 'Comfortaa-Bold', android: 'Comfortaa Bold' }),
  },
});
```

## Credits

This document was inspired by and writted based on [React Native Custom Fonts by Dave Hudson](https://medium.com/react-native-training/react-native-custom-fonts-ccc9aacf9e5e).
