# Release Notes

November 22 2022

## V_1.4.9 (SDK version)

## What's New

- When the parent app has passed true for the fi_callback parameter, if the user clicks on a web link such as the "Privacy Policy" link on the Zelle UI, then the getValue method will be triggered and pass "privacy policy" as the value for the name parameter. The parent app handles this callback on their side.

## Updates

- Generic Callback Update
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

Zelle zelle = new Zelle(
fi_callback : true ** fi_callback is true when handling getValue method otherwise false **
); 
```

## Build

[ZelleSDK_V_1.4.9.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11576732/ZelleSDK_V_1.4.9.aar.zip)

