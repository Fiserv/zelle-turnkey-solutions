# Release Notes

March 22 2022

## v1.3.4 (api version)

## What's New

- ZelleSDK supports minimum deployment target 13.
- Added few more parameters such baseUrl, version in Zelle constructor parameter.
- Implemented new features such as Session timeout, contact callback etc.

## Enhancements

- ZelleSDK Supports iOS 13.
- Objective C/ Swift.
- Contact callback methods changed as per UI requirement.
- BaseUrl parameter enabled.
- New enhancement: Device info, Version, SessionTimeout.

###  Zelle Constructor

```json
private let zelle = Zelle(
applicationName: "Demo Bank", // Optional 
baseURL: " ",
institutionId: " ",
product = "zelle",
ssoKey: " ",
parameters: [
"key1" : "value1",
"key2" : "value2",
"key3" : "value3"
] // Optional (Nullable) unless Zelle is accessed through Bill Pay 

) 
```

## Builds


