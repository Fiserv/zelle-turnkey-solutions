# Release Notes

July 15 2022

## V_2.0.5 (SDK version)

## What's New

- FI can customize the prominent disclosure popup title and message for Contact permission, which will
  trigger before accessing the native Contact permission alert.

## Updates

- Prominent Disclosure implementation for Contact Permission
- Notification bar color fix
- Notification bar color fix
- ZelleSDK theme issue fix

### Prominent Disclosure Initialization

```json
val pdContact = mapOf(
"title" to "Contact Title",
"message" to "Contact Message")

val appData = mapOf(
"pd_contact" to pdContact )
``` 

### Prominent Disclosure Implementation

```json
val zelle = Zelle(
appData = appData
)
``` 

## Build

[ZelleSDK_V_2.0.5.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11590553/ZelleSDK_V_2.0.5.aar.zip)



