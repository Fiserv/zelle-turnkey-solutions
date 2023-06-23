# Steps for Quick Start

## Project Setup

### 1. Add ZelleSDK.aar to your project libs folder:

![library_folder](../../assets/images/img_lib_folder.png)

### 2. Add the following line to the app module Gradle file dependencies:

```json
implementation files ('libs/ZelleSDK.aar')
implementation 'androidx.core:core-ktx:1.5.0'
implementation 'androidx.appcompat:appcompat:1.3.0'
implementation 'com.google.android.material:material:1.3.0'
implementation 'androidx.constraintlayout:constraintlayout:2.0.4'

//QR Code reader
implementation('com.journeyapps:zxing-android-embedded:4.2.0') {transitive = false}
implementation 'com.google.zxing:core:3.3.3' 
```

### 3. Import ZelleSDK where needed in any source code file:

```json
import com.fiserv.dps.mobile.sdk.bridge.model.* 
import com.fiserv.dps.mobile.sdk.bridge.controller.Bridge 
```

### 4. Device Orientation Support (Optional)

Add the configChanges below to the launching ZelleLaunchingActivity class in the Manifest file:

```json
android:configChanges="keyboardHidden|orientation|screenSize|layoutDirection|uiMode" 
```

### 5. Create a Zelle bridge configuration object.

```json
HashMap<String, String> params = new HashMap<String, String> (); 
params.put("key1", "value1"); 
params.put("key2", "value2"); 
params.put("key3", "value3"); 

HashMap<String, String> loaderData = new HashMap<String, String> (); 
loaderData.put("loaderColor", "hex color"); 
loaderData.put("bgColor", "hex color"); 

Map<String, String> pdContact= new HashMap<String, String> (); 
pdContact.put("title", "We would like to access your phone contacts"); //prominent disclosure title 
pdContact.put("message", "We only sync phone numbers and email addresses from your contact list to help you add and pay a new recipient in Zelle®"); //prominent disclosure message 

Map<String, String> pdCamera= new HashMap<String, String> (); 
pdCamera.put("title", "We would like to access your camera"); //prominent disclosure title 
pdCamera.put("message", "We only access your camera to help you add and pay a new recipient in Zelle®"); //prominent disclosure message 

Map<String, String> pdPhotos= new HashMap<String, String> (); 
pdPhotos.put("title", "We would like to access your photos"); //prominent disclosure title 
pdPhotos.put("message", "We only access your photos to help you add and pay a new recipient in Zelle®"); //prominent disclosure message 

Map<String, Map<String, String>> appData = new HashMap<String, HashMap<String, String>> (); 

appData.put(“pd_contact”, pdContact); 
appData.put(“pd_camera”, pdCamera); 
appData.put(“pd_gallery”, pdPhotos); 

 
Zelle zelle = new Zelle( 
   "Demo Bank", //applicationName (Optional) 
   "https://certtransfers.fta.cashedge.com/popnet/faces/loginServlet", //baseURL  
   "88850000", //institutionId 
   "zelle", //product 
   "e78abf35705a6d9b51fbf3939aa82489", // ssoKey 
   true, //fi_callback is true when handling getValue method otherwise false 
   loaderData, //Optional (Nullable) 
   appData, //Optional (Nullable) 
   params //Optional (Nullable) unless Zelle is accessed through Bill Pay 
); 
```

Note: 

- Since applicationName, loaderData, and appData are optional parameters, you may pass null (e.g., appData: null).
- params is an optional parameter unless Zelle® is accessed through Bill Pay. If Zelle® is accessed through Bill Pay, the name value pair flowtype=BillPay is required. Otherwise, you may pass null (e.g., params: null).
- ZelleSDK will automatically add the appropriate default values for these parameters to the URL: product.version and container (mobile_sdk_android).

### 6. Create a Bridge object (optional lazy implementation).

```json
Bridge bridge = new Bridge(this, zelle); 
```

- Note: Pass the appropriate parent appActivity (type Activity) to activity.

### 7. Launch Zelle® inside another view using code initialization (Recommended)

If replacing an existing WebView, replace existing WebView with BridgeView.

```json
// optionally: set the contact pre-caching flag (default: false) 
zelle.preCacheContacts = true

BridgeView zelleView = bridge.view();
//user supportFragmentManager to push zelleView to desired location  
```

### Launch Zelle® as a popup (not inside another view)

```json
// optionally: set the contact pre-caching flag (default: false) 
zelle.preCacheContacts = true

BridgePopup zellePopup = bridge.popup();
popup.show(supportFragmentManager, zellePopup.getTag());  
```

### Session Timeout and Intercepting Web Links

```json
//Implement the GenericTag with Activity/Fragment
public class ZelleLaunchFragment extends Fragment implements GenericTag{
  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState){
    //Initialize generic tag
    Bridge.genericTag = this
    return inflater.inflate(R.layout.fragment_zelle_launch, container, false)
  }
}

//Inside the class override the sessionTag method & getValue method
@Override
public void sessionTag (String name) {
  if (name == “Landing”) {
    //Here navigates the application to the desired screen. (This function will be triggered after the session expires)  
  }
}

// If the parent app has passed true for the fi_callback parameter, if the user  
// clicks on a web link such as the "Privacy Policy" link on the Zelle UI, then  
// the getValue method will be triggered and pass "privacy policy" as the value  
// for the name parameter. The parent app handles this callback on their side. 

@Override
public void getValue(String name) {
  if (name == “TAG_NAME”) {
    //Here navigates the application to the desired screen. (This function will help to communicate between Zelle UI and parent app)  
  }
}  
```

### Supported Versions

- Minimum SDK: 24
- Minimum OS: Android 7.0 Nougat

### Zelle Mobile SDK Size

224 KB

### Dependency

zxing:core - 3.3.3 ([QR Code library](https://github.com/journeyapps/zxing-android-embedded)) 

### Sample Project

[Zelle Payment.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11689585/Zelle.Payment.zip)
