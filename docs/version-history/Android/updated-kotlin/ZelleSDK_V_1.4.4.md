# Release Notes

August 03 2022

## V_1.4.4 (SDK version)

## What's New

- When the Zelle UI session has ended, the Zelle SDK will call the FI app's session timeout callback function.

## Enhancements

- Custom callback method was implemented for session timeout.

### Session Timeout Implementation

```json
*** Implement the GenericTag with Activity/Fragment ***

Class ZelleLaunchFragment: Fragment (), GenericTag{
override fun onCreateView(inflater: LayoutInflater, container ViewGroup?, savedInstanceState: Bundle?) {
_binding = FragmentZelleLaunchBinding.inflater.inflate(inflater, container, false)
//Initialize generic tag 
genericTag = this
return binding.root
}
}

*** Inside the class override the sessionTag method ***
override fun sessionTag(name: String) {
if (name == “Landing”){
  *** Here navigates the application to the desired screen. (This function will be triggered after the session expires) ***  
}
} 
```

## Build

[ZelleSDK_V_1.4.4.aar.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11590465/ZelleSDK_V_1.4.4.aar.zip)
