# Release Notes

August 11 2022

## V_2.0.7 (SDK version)

## What's New

- FI can customize the prominent disclosure popup title and message for Contact, Camera and Gallery permission, which will
  trigger before accessing the native contact permission alert.

## Updates

- Prominent Disclosure implementation for Contact, Camera and Gallery Permission
- Custom callback method was implemented for session timeout
- Prominent Disclosure implementation for Contact Permission
- Notification bar color fix
- ZelleSDK theme issue fix

### Prominent Disclosure Initialization

```json
val pdContact = mapOf(
"title" to "Contact Title",
"message" to "Contact Message")

val pdCamera = mapOf(
"title" to  "Camera Title",
"message" to "Camera Message")


val pdPhoto = mapOf(
"title" to  "Photo Title",
"message" to "Photo Message")

val appData = mapOf(
"pd_contact" to pdContact,
"pd_camera" to pdCamera,
"pd_gallery" to pdPhoto)
``` 

### Prominent Disclosure Implementation

```json
val zelle = Zelle(
appData = appData
)
```

## Build

[ZelleSDK_V_2.0.7.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11590541/ZelleSDK_V_2.0.7.aar.zip)




