# Steps for Quick Start

## Project Setup

### 1. Open the file info.plist (right-click > Open As > Source Code). Add the following keys to the file:

```json
<key>NSContactsUsageDescription</key>
<string>[PERMISSION_DESCRIPTION]</string>

<key>NSCameraUsageDescription</key>
<string>[PERMISSION_DESCRIPTION]</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>[PERMISSION_DESCRIPTION]</string> 
```

### 2. Import ZelleSDK.xcframework for the required target under project settings (Project > Target > Frameworks, Libraries, and Embedded Contents).

### 3. Set ZelleSDK.xcframework to “Embed & Sign”:

![ios_embed](https://github.com/Fiserv/zelle-turnkey-solutions/assets/114585394/88065cc8-3184-4fe5-9ba3-2a25eef452c5)

![ios_embed sign](https://github.com/Fiserv/zelle-turnkey-solutions/assets/114585394/a1668e2e-efae-4ba6-9cbb-39410196f082)

### 4. Import ZelleSDK as needed in any source code files. 

### 5. Create a Zelle bridge configuration object. 

```json
Zelle* zelle = [[Zelle alloc] initWith
  applicationName:@"Demo Bank" // Optional 
  baseURL:@"https://certtransfers.fta.cashedge.com/popnet/faces/loginServlet"
  institutionId:@"88850000"
  product:@"zelle"
  ssoKey:@"e78abf35705a6d9b51fbf3939aa82489"
  fi_callback:true //true when handling getValue method otherwise false/nil 

  loaderData:@{
    @"loaderColor": @"hex color",
    @"bgColor": @"hex color"} // Optional (Nullable) 
  parameters:@{
    @"key1": @"value1",
    @"key2": @"value2",
    @"key3": @"value3"}]; // Optional (Nullable) unless Zelle is accessed via Bill Pay 
```

#### Note: 
- Since loaderData is an optional parameter, you may pass nil or empty (e.g., loaderData: nil). 

- Parameters is an optional parameter unless Zelle® is accessed through Bill Pay. If Zelle® is accessed through Bill Pay, the name value pair flowtype-BillPay is required. Otherwise, you may pass nil or empty (e.g., parameters: nil or parameters: @{}.

- ZelleSDK will automatically add the appropriate default values for these parameters to the URL: product.version and container (mobile_sdk_ios). 

### 6. Create a Bridge object (optional lazy implementation). 

#### Note: 
- Pass the appropriate parent ViewController (type UIViewController) to self. 

```json
Bridge* bridge = [[Bridge alloc]
initWithConfig:zelle viewController:self ]; 
```

### 7. Launch Zelle® inside another view using code initialization (Recommended).

```json
CGRect zelleFrame = CGRectMake(0, 0, self.view.bounds.size.width, self.view.bounds.size.height);
UIView* zelleView = [bridge viewWithFrame:zelleFrame];
[view addSubview:zelleView]; 
```

### 8. Set the GenericTagDelegate Protocol delegate to the existing ViewController.

```json
Bridge.genericTag = self 
```

### 9. Launch Zelle® inside another view and fill it (for screen orientation).

```json
BridgeView* bridgeView = [bridge view];
[bridgeView fill:self.view]; 
```

### 10. Launch Zelle® as a popup (not inside another view).

```json
BridgePopup* bridgePopup = [bridge popup];
[bridgePopup anchorTo:self.view]; 
```

### 11. Session Timeout and Intercepting Web Links.

```json
//Implement the GenericTagDelegate with ParentViewController 

Class ZelleViewController: UIViewController, GenericTagDelegate{



//Inside the class override the sessionTag & getValue method 

-(void)sessionTagWithName:(NSString*) name {
if ([name isEqualToString: @ “Landing”]) {
//Here navigates the application to the desired screen. (This function will be triggered after the session expires)  
}
}

// If the parent app has passed true for the fi_callback parameter, if the user  
// clicks on a web link such as the "Privacy Policy" link on the Zelle UI, then  
// the getValue method will be triggered and pass "privacy policy" as the value  
// for the name parameter. The parent app handles this callback on their side. 

-(void)getValueWithName:(NSString*) name {
if ([name isEqualToString: @ “TAG_NAME”]) {
//Here navigates the application to the desired screen. (This function will help to communicate Zelle UI and parent app)  
}
}

} 
```

### 12. Supported Versions:

- Minimum Xcode Version: 11
- Minimum OS: iOS 13

### 13. Zelle Mobile SDK Size:

2.6 MB (size includes simulator)

### 14. Dependency:

QRCodeReader - 10.1.0 (QR Code library) 
