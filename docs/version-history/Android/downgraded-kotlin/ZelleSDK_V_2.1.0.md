# Release Notes

October 21 2022

## V_2.1.0 (SDK version)

## What's New

- This feature will make it easier for Zelle UI and the parent app to open the desired screen out of Zelle Screen.

## Updates

- Custom Callback Implemented
- WebView height and width were fixed
- Implemented the AppCompat and Material Themes
- Prominent Disclosure implementation for Contact, Camera and Gallery Permission
- Custom callback method was implemented for session timeout
- Prominent Disclosure implementation for Contact Permission
- Notification bar color fix
- ZelleSDK theme issue fix

### Generic Callback Implementation

```json
Class ZelleLaunchFragment: Fragment (), GenericTag{
override fun onCreateView(inflater: LayoutInflater, container ViewGroup?, savedInstanceState: Bundle?) {
_binding = FragmentZelleLaunchBinding.inflater.inflate(inflater, container, false)
        
***Initialize generic tag***
genericTag = this
        
return binding.root
}
}

override fun getValue(name: String) { 
    if (name == “TAG_NAME”) {
        ***Here navigates the application to the desired screen. (This function will help to communicate between Zelle UI and parent app)***
    } 
} 
```

## Build

[ZelleSDK_V_2.1.0.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11576758/ZelleSDK_V_2.1.0.aar.zip)
