# Release Notes

July 15 2022

## V_1.4.3 (SDK version)

## What's New

- FI can customize the prominent disclosure popup title and message for contact permission, which will
  trigger before accessing the native contact permission alert.

## Enhancements

- Prominent Disclosure implementation for Contact Permission

### Prominent Disclosure Initialisation

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

