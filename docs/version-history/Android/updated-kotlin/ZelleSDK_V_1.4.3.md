# Release Notes

July 15 2022

## V_1.4.3 (SDK version)

## What's New

- FI can customize the prominent disclosure popup title and message for Contact permission, which will
  trigger before accessing the native Contact permission alert.

## Enhancements

- Prominent Disclosure implementation for Contact Permission

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

- [ZelleSDK_V_1.4.3](?path=docs/builds/Android/upgraded-kotlin/ZelleSDK_V_1.4.3.aar)

