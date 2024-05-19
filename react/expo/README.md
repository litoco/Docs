# REACT NATIVE EXPO DOC

### My Findings

##### 1. Expo's dev-client and config-plugin
Lets say we need to use camera of the device and it is a native feature that isn't available in defult expo SDK:
* [Expo Dev client](https://docs.expo.dev/develop/development-builds/introduction/#what-is-expo-dev-client) is used to build another version of expo go app that supports the native feature like camera feature in our case.
* [Expo config plugin](https://docs.expo.dev/config-plugins/introduction/) is used to configure necessary dependecies and permissions required for the feature to work on the newly built expo go app. In our case it will configure necessary dependecies and permission to use camera feature in expo go application.


In summary, Expo Config Plugins set ups the necessary native dependencies and configurations, and the Expo Dev Client builds a custom Expo Go application that includes these native features, enabling development and testing of your app with those features. So when you build you application using:
```
expo build:android
expo build:ios
```
it will ensure that the native feature works on the both of these native builds.
