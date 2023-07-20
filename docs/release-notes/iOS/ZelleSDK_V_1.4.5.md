# Release Notes

March 30 2023

## V_1.4.5 (SDK version)

## What's New

- Custom Loader: FI can customise the loader colour and the background colour while launching Zelle.

## Updates

- Custom loader implementation
- Memory leak that was fixed
- iOS 14 gallery permission updates
- App domain for iOS 14 to whitelist the url
- Custom Callback implemented
- WKUIDelegate method implementation
- WebView height and width were fixed
- Custom callback method added for session timeout
- Camera cancel button issue fixed
- Removed location permission
- Popup dismissal
- Device Orientation
- QRCode issue fix
- ZelleSDK Supports iOS 13
- Objective C/ Swift
- Contact callback methods changed as per UI requirement
- BaseUrl parameter enabled
- New enhancement: Device info, Version, SessionTimeout

### Custom Loader Implementation

```json
private let zelle = Zelle(
loaderData: [
"loaderColor" : "hex color",
"bgColor" : "hex color",
], // Optional (Nullable)
) 
```

## Build

[ZelleSDK_V_1.4.5.xcframework.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11599226/ZelleSDK_V_1.4.5.xcframework.zip)

## Deprecated

- ZelleSDKC_V_1.4.4
- ZelleSDKB_V_1.4.4

## Supporting platforms

- [Swift](?path=docs/supporting-documents/swift.md)
- [Objective C](?path=docs/supporting-documents/objectivec.md)
- [Cordova](?path=docs/supporting-documents/cordova.md)
- [Flutter](?path=docs/supporting-documents/fultter.md)
- [Kony](?path=docs/supporting-documents/kony.md)
- [React Native](?path=docs/supporting-documents/react-native.md)
- [Xamarin](?path=docs/supporting-documents/xamarin.md)
