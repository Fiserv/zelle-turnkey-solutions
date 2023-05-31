# Release Notes

October 21 2022

## v1.4.1 (api version)

## What's New

- This feature will make it easier for Zelle UI and the parent app to open the desired screen out of Zelle Screen.

## Enhancements

- Custom Callback implemented

### Generic Callback Implementation

```json
** Set the GenericTagDelegate Protocol delegate to the existing ViewController **
Bridge.genericTag = self

private let zelle = Zelle(
fi_callback: true, //Mandatory when handling getValue method otherwise optional
)

//Implement the GenericTagDelegate with ParentViewController 

Class ZelleViewController: UIViewController, GenericTagDelegate{

 ** If the parent app has passed true for the fi_callback parameter, if the user clicks on a web link such as the "Privacy Policy" link on the Zelle UI, then the getValue method will be triggered and pass "privacy policy" as the value for the name parameter. The parent app handles this callback on their side ** 

func getValue(name: String) {
if (name == “TAG_NAME”) {
** Here navigates the application to the desired screen. (This function will help to communicate with the Zelle UI and parent app) **
}
}

} 
```

## Build

[ZelleSDK_V_1.4.1.xcframework.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11609738/ZelleSDK_V_1.4.1.xcframework.zip)


