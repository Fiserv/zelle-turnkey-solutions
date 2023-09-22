# Release Notes

February 27 2023

## v1.4.4 (api version)

## What's New

- ZelleSDK creates instances of objects which is retained in memory even after ZelleSDK is killed/logged out. To avoid this, ZelleSDK creates weak references between objects so that objects gets released once ZelleSDK is killed/logged out.

## Updates

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

## Build

[ZelleSDK_V_1.4.4.xcframework.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11609694/ZelleSDK_V_1.4.4.xcframework.zip)



