# Release Notes

June 24 2022

## v1.3.8 (api version)

## What's New

- When the Zelle UI session has ended, the Zelle SDK will call the FI app's session timeout callback function.

## Enhancements

- Custom callback method added for session timeout.

### Session Timeout Implementation

```json
** Set the GenericTagDelegate Protocol delegate to the existing ViewController **

Bridge.genericTag = self

** Implement the GenericTagDelegate with ParentViewController **

Class ZelleViewController: UIViewController, GenericTagDelegate{
        
** Inside the class override the sessionTag **
        
func sessionTag(name: String){
if (name == “Landing”) {
//Here navigates the application to the desired screen. (This function will be triggered after the session expires)  
}
}
}
```

## Build

[ZelleSDK_V_1.3.8.xcframework.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11609750/ZelleSDK_V_1.3.8.xcframework.zip)


