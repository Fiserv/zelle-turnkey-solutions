# Release Notes

April 30 2023

## Kotlin Version

1.3.72

## V_2.1.3 (SDK version)

## What's New

- FI can customize the loader color and it's background during the loading time.

## Updates

- Custom loader implementation
- Memory leak that was fixed
- Prominent Disclosure popup issue fix
- Generic Callback Update
- Custom Callback Implemented
- WebView height and width were fixed
- Implemented the AppCompat and Material Themes
- Prominent Disclosure implementation for Contact, Camera and Gallery Permission
- A custom callback method was implemented for session timeout
- Prominent Disclosure implementation for Contact Permission
- Notification bar color fix
- ZelleSDK theme issue fix

### Custom Loader Implementation

```json
val zelle = Zelle(
loaderData = mapOf(
"loaderColor" to "#ffffff",
"bgColor" to "#747474"
)
)
```

## Build

[ZelleSDK_V_2.1.3.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11576711/ZelleSDK_V_2.1.3.aar.zip)

## Deprecated

- ZelleSDKC_V_2.1.2
- ZelleSDKB_V_2.1.2

## Supporting platforms

- [Kotlin](?path=docs/supporting-documents/kotlin.md)
- [Java](?path=docs/supporting-documents/java.md)
- [Cordova](?path=docs/supporting-documents/cordova.md)
- [Flutter](?path=docs/supporting-documents/flutter.md)
- [Kony](?path=docs/supporting-documents/kony.md)
- [React Native](?path=docs/supporting-documents/react-native.md)
- [Xamarin](?path=docs/supporting-documents/xamarin.md)
