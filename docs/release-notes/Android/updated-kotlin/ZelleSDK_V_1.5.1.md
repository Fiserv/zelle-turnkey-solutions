# Release Notes

April 30 2023

## Kotlin Version

1.5.10

## V_1.5.1 (SDK version)

## What's New

- Custom Loader: FI can customise the loader colour and the background colour while launching Zelle.

## Updates

- Custom loader implementation
- Memory leak that was fixed
- Prominent Disclosure popup issue fix
- Generic Callback Update
- Custom Callback Implemented
- WebView height and width were fixed
- Implemented the AppCompat and Material Themes
- Prominent Disclosure implementation for Contact, Camera and Gallery Permission
- Custom callback method was implemented for session timeout
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

#### Build

[ZelleSDK_V_1.5.1.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11576725/ZelleSDK_V_1.5.1.aar.zip)

## Deprecated

- ZelleSDKC_V_1.5.0
- ZelleSDKB_V_1.5.0

## Supporting platforms

- [Kotlin](?path=docs/supporting-documents/kotlin.md)
- [Java](?path=docs/supporting-documents/java.md)
- [Cordova](?path=docs/supporting-documents/cordova.md)
- [Flutter](?path=docs/supporting-documents/flutter.md)
- [Kony](?path=docs/supporting-documents/kony.md)
- [React Native](?path=docs/supporting-documents/react-native.md)
- [Xamarin](?path=docs/supporting-documents/xamarin.md)
