# Release Notes

October 21 2022

## V_1.4.8 (SDK version)

## What's New

- This feature will make it easier for Zelle UI and the parent app to open the desired screen out of Zelle Screen.

## Fixed

- Generic Callback Update.

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

[ZelleSDK_V_1.4.8.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11576744/ZelleSDK_V_1.4.8.aar.zip)
