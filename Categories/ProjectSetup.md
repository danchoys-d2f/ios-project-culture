[< Back](../README.md)

# Project setup

iOS projects can be set up in many different ways. Here we will cover some tips that are used in our iOS projects.

## Table of contents

* [Multi-environment setup](#multi-envoronment-setup)
  * [Conditional resources](#conditional-resources)
  * [Conditional compilation](#conditional-compilation)
  * [Conditional Info.plist values](#conditional-infoplist-values)
* [Dependency management](#dependency-management)
* [Custom configs](#custom-configs)
* [Code signing](#code-signing)

## Multi-environment setup

Often we need to have separate configurations in the same project. Most frequently this happens because of having multiple backends - one used for staging and another one for production.

While there are multiple ways to achieve this goal, we will focus on the recommended and most straight-forward one: through Xcode configurations or `xcconfig`.

### Conditional resources

To have Xcode build your app with a specific version of some resource based on the configuration, you need to do the following:

1. Declare a custom build setting (lets call it `SETTINGS_PLIST_PATH`).
1. Set different values for different configurations in Xcode (e.g. `$(PROJECT_DIR)/$(TARGET_NAME)/Resources/Config/Settings-dev.plist`).
1. Add a `Run Script` build phase to the project and put the following code there:

`cp -r "${SETTINGS_PLIST_PATH}/" "${BUILT_PRODUCTS_DIR}/${PRODUCT_NAME}.app/Settings.plist"`

### Conditional compilation

One of the most useful features is conditional compilation. With this technique only the code you want will find its way into the binary. Lets say you need to have some feature enabled in debug configurations, but disabled in production. Here are the steps:

1. Define a new custom build setting and call it e.g. `MY_FEATURE_STATUS`.
1. Set this setting to either `ENABLED` or `DISABLED` based on the configuration.
1. Add a new compilation flag to `Other Swift Flags` in the form of `-D MY_FEATURE_$(MY_FEATURE_STATUS)`.
1. Use the following construction in the code:

```swift
#if MY_FEATURE_ENABLED
    // Do stuff to enable your feature
#endif
```

You can easily extend this example to having enumeration values if you specify these values in place of `ENABLED` and `DISABLED`:

```swift
#if MY_FEATURE_CASE_ONE
    // Do whatever makes sense in the first case
#elseif MY_FEATURE_CASE_TWO
    // Do whatever makes sense in the second case
#elseif MY_FEATURE_CASE_THREE
    // Do whatever makes sense in the third case
#else
    // Execute the fallback logic
#endif
```

### Conditional Info.plist values

All the build settings are available as placeholders in `Info.plist`. You just need to specify it in the form of `$(BUILD_SETTING_NAME)` and it will be replaced with the corresponding value at build time. This way you can set different bundle identifiers, app names and other properties based on the selected configuration.

## Dependency management

In our projects we are using [CocoaPods](http://cocoadocs.org) to manage the dependencies. When dealing with Pods there are several things to keep in mind:

* Do not include __Pods__ directory into .gitignore.
* Always specify complete versions for the pods you are adding.
* Use `!inhibit_all_warnings` config to hide warnings, related to third-party libraries.

## Custom configs

In iOS projects custom configuration can be stored in multiple ways. Depending on the amount and sensitivity of this data you are going to choose one of the following:

* __Info.plist__

  Best for small non-sensitive chunks of data, most often a simple string. Don't overuse this approach as it often leads to the rapid growth of the file. Apart from this the values that you get in runtime are not strongly typed which makes it less convenient to use.

* __Resource files (plist, strings, etc)__

  Suitable for organised non-sensitive data, like a list of countries. Custom types can be initialised with plists (not without specific tools though), which makes them a better alternative to Info.plist.

* __Code__

  Best for sensitive data, like API keys, credentials and URLs. Can be further obfuscated to improve the security. Additional advantage is type safety and compile time checks.

## Code signing

[TBD]

## Credits

Ⓒ Devoteam Digital Factory 2018
