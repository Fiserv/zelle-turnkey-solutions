# Steps for Quick Start

## Creating POC (Proof of Concepts) with React Native

### Prerequisites:

Nodejs, react-native-cli or expo-cli, Android emulator or real device, iOS simulator or real device

### 1. Create new project (ReactDemo) using Terminal

```json
 npx create-expo-app ReactDemo
```

### 2. Navigate to project directory

```json
cd ReactDemo
```

### 3. Start the project

```json
npm start
```

Note: You can also use: npx expo start

### 4. Add android/iOS package and bundle name in app.json file

```json
"ios": {
     "supportsTablet": true,
     "bundleIdentifier": "your_package_name",
     "buildNumber": "1.0.0"
   }, 

   "android": {
     "adaptiveIcon": {
       "foregroundImage": "./assets/adaptive-icon.png",
       "backgroundColor": "#FFFFFF"
     },
     "package": "your_package_name",
     "versionCode": 1
   } 
```

### 5. Generate android and iOS folder

```json
expo eject
```

Note: Now you will get android/ios folders in your project folder.

### 6. Launch the application

```json
expo start
```

Note: This step ensures that the Metro bundler is running and serving your app's JS bundle for development. Leave this running and continue with the following steps.

### 7. Running android

Open the android directory in Android Studio, then build and run the project on an Android device or emulator.

### 8. Running iOS project

Note: For iOS, navigate to the ios folder and run "npx pod-install" to link the native iOS packages.  Now you will get "xcworkspace" in the ios folder.

Open "xcworkspace" using Xcode to build, install, and run the project on your test device or simulator.

## Integrating Android ZelleSDK with POC

### 1. Add ZelleSDK.aar to your project libs folder:

Open the android folder in Android Studio and add the ZelleSDK.aar file in android/app/libs.

### 2. Create MyModule.java class

Create the file MyModule.java inside android/app/src/main/java/com/your-app-name/ and then add the following content:

```json
import android.content.Intent;
import android.util.Log;
import androidx.annotation.NonNull;
import com.facebook.react.bridge.Callback;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;
import com.facebook.react.bridge.ReadableMap;
import com.fiserv.dps.mobile.sdk.bridge.controller.Bridge;
import com.fiserv.dps.mobile.sdk.interfaces.GenericTag; 

//Implementing GenericTag for session timeout
public class MyModule extends ReactContextBaseJavaModule implements GenericTag { 
        
    ReactApplicationContext context = getReactApplicationContext(); 
        
    public MyModule(@NonNull ReactApplicationContext reactContext) {
        super(reactContext);
    } 
            
   Boolean fi_callback = false;   
        
    @NonNull
    @Override
    public String getName() {
        return "MyModule";
    }  

    @ReactMethod
    public void NavigateToZelle(String appName, String baseURL, String instId, String product, Boolean fi_callback, String ssoKey, ReadableMap loaderData, ReadableMap parameters, ReadableMap pd){ 

        this.fi_callback = fi_callback;
        Bridge.genericTag = this;
        Intent intent = new Intent(context, LaunchZelleActivity.class);
        if (intent.resolveActivity(context.getPackageManager()) != null){ 

            intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            intent.putExtra("appName", appName);
            intent.putExtra("baseURL", baseURL);
            intent.putExtra("instId", instId);
            intent.putExtra("product", product);
            intent.putExtra("ssoKey", ssoKey);
            intent.putExtra(" fi_callback ", fi_callback);
            intent.putExtra("loaderData", loaderData.toHashMap());
            intent.putExtra("parameters", parameters.toHashMap());
            intent.putExtra("pd", pd.toHashMap());
            context.startActivity(intent);
        }
    } 
                    
    @Override
    public void sessionTag(@NonNull String s) {//override method for session timeout
        if (!s.equals("")){
         try {
                WritableMap event = Arguments.createMap();
                event.putString("data", s);
                context.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
                        .emit("sessionTimeout", event);
            } catch (Exception e){
                Log.e("Exception: " + e.getMessage());
            }
        }
    }

   @Override
    public void getValue(@NonNull String s) {//override method for custom callback
        
        if (!s.equals("")){
            try {
                WritableMap event = Arguments.createMap();
                event.putString("data", s);
                context.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
                        .emit("getValue",  event);
            } catch (Exception e){
                Log.e("Exception: " + e.getMessage());
            }
        }
    }
} 
```

### 3. Add your Native Module to ReactPackage

First create a new Java Class named MyPackage.java that implements ReactPackage inside the android/app/src/main/java/com/your-app-name/, then add the following content:

```json
import androidx.annotation.NonNull;
import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List; 
        
public class MyPackage implements ReactPackage {
        
    @NonNull
    @Override
    public List<NativeModule> createNativeModules(@NonNull ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new MyModule(reactContext));
        return modules;
    } 
        
    @NonNull
    @Override
    public List<ViewManager> createViewManagers(@NonNull ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }
} 
```

### 4. Create LaunchZelleActivity.kt class

Create LaunchZelleActivity.kt and add the following content:

```json
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import com.fiserv.dps.mobile.sdk.bridge.controller.Bridge
import com.fiserv.dps.mobile.sdk.bridge.model.Zelle 

class LaunchZelleActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_launch_zelle)

        val appName = intent.getStringExtra("appName").toString()
        val baseUrl = intent.getStringExtra("baseURL").toString()
        val instId = intent.getStringExtra("instId").toString()
        val product = intent.getStringExtra("product").toString()
        val ssoKey = intent.getStringExtra("ssoKey").toString()
        val fi_callback: Boolean = intent.extras?.getBoolean("fi_callback") ?: false
        val loaderData: HashMap<String, String> = intent.getSerializableExtra("loaderData") as HashMap<String
        val parameters: HashMap<String, String> = intent.getSerializableExtra("parameters") as HashMap<String, String>
        val pd: Map<String, Map<String, String>> = intent.getSerializableExtra("pd") as Map<String, Map<String, String>>
        
        val zelle = Zelle(
            appName,
            baseUrl,
            instId,
            product,
            ssoKey,
            fi_callback,
            loaderData,
            pd,
            parameters
        ) 

        val bridge = Bridge(this, zelle)
        zelle.preCacheContacts = true
        val view = bridge.view()
        supportFragmentManager.beginTransaction().replace(R.id.frame_layout, view).commit()
    }
} 
```

### 5. Create layout.activity_launch_zelle.xml

Create the layout.activity_launch_zelle.xml file and add the following content:

```json
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".LaunchZelleActivity"> 

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/frame_layout"
        android:foregroundGravity="center"/> 

</androidx.constraintlayout.widget.ConstraintLayout> 
```

### 6. Add dependencies in app level build.gradle

Add these dependencies in the app level build.gradle file:

```json
 implementation fileTree(dir: "libs", include: ["*.jar","*.aar"])    
 implementation 'androidx.appcompat:appcompat:1.3.0'    
 implementation 'androidx.constraintlayout:constraintlayout:2.0.4'//qr code reader    
 implementation('com.journeyapps:zxing-android-embedded:4.3.0') { transitive = false}    
 implementation 'com.google.zxing:core:3.4.0'
```

### 7. Add permissions in AndroidManifest.xml

```json
<uses-permission android:name="android.permission.INTERNET" /> 
<uses-permission android:name="android.permission.CAMERA"/> 
```

Note: Minimum SDK version is 24.

## Integrating iOS ZelleSDK with POC

### 1. Add ZelleSDK.xcframework  to your project folder:

Open "xcworkspace" using Xcode and add ZelleSDK.xcframework in ios/ReactDemo.

### 2. Create MyModule.m class

Create the file MyModule.m and add the following content:

```json
#import <Foundation/Foundation.h>
#import "React/RCTBridgeModule.h" 
        
@interface RCT_EXTERN_MODULE(MyModule, NSObject)
        
RCT_EXTERN_METHOD(NavigateToZelle: (NSString *)appName baseUrl:(NSString *)baseUrl instId:(NSString *)instId product:(NSString *)product ssoKey:(NSString *)ssoKey  fi_callback:(BOOL *)fi_callback  loaderData:(NSDictionary *)loaderData parameters:(NSDictionary *)parameters pd:(NSDictionary *)pd callback:(RCTResponseSenderBlock)callback) 

@end 
```

### 3. Create MyModule.swift class

Create the file MyModule.swift and add the following content:

```json
import Foundation
import UIKit
import ZelleSDK
import UIKit
import React.RCTBridgeModule
import React 

@objc(MyModule)
class MyModule: RCTEventEmitter, GenericTagDelegate{//Implement GenericTagDelegate to handle session timout 
        
  override static func requiresMainQueueSetup() -> Bool {
    return true
  } 
        
  override func supportedEvents() -> [String]! {
    return ["sessionTimeout","getValue"]
  } 
          
  @objc func NavigateToZelle(_ appName: String, baseUrl: String, instId: String, product: String, ssoKey:String,  fi_callback: Bool, loaderData: NSDictionary , parameters: NSDictionary, pd: NSDictionary, callback: @escaping RCTResponseSenderBlock){ 

    Bridge.genericTag = self 
   
    DispatchQueue.main.async {
      let storyboard = UIStoryboard(name: "main", bundle: nil)
      let secondVC: ZelleViewController = storyboard.instantiateViewController(withIdentifier: "ZelleViewController") as! ZelleViewController
      secondVC.applicationName = appName
      secondVC.baseUrl = baseUrl
      secondVC.institutionId = instId
      secondVC.product = product
      secondVC.ssoKey = ssoKey
      secondVC.fi_callback = fi_callback
      secondVC.loaderData = loaderData
      secondVC.parameters = parameters
       UIApplication.shared.keyWindow?.rootViewController?.present(secondVC, animated: true, completion: nil)
    }
  } 
        
  func sessionTag(name: String) {
    if(name != ""){
      self.sendEvent(withName: "sessionTimeout", body: ["data":name])
    }
  }

  func getValue(name: String) {
    if(name != ""){
      self.sendEvent(withName: "getValue", body: ["data":name])
    }
  }
}
```

### 4. Create ZelleViewController.swift class

Create ZelleViewController.swift and add the following content:

```json
import UIKit
import ZelleSDK 

class ZelleViewController: UIViewController { 
        
    @IBOutlet weak var viewContainer: UIView!
        
    var applicationName = ""
    va baseUrl = ""
    var institutionId = ""
    var product = ""
    var ssoKey = ""
    var fi_callback = false
    var loaderData: NSDictionary?
    var parameters: NSDictionary? 
        
    override func viewDidLoad() {
      super.viewDidLoad() 
        
      let zelle = Zelle(
          applicationName : applicationName,
          baseURL: baseUrl,
          institutionId: institutionId,
          product: product,
          ssoKey: ssoKey,
          fi_callback: fi_callback,
          loaderData: loaderData! as! [String : String]
          parameters: parameters! as! [String : String]
      ) 
              
      var bridge: Bridge = {
        Bridge(
          config: zelle,
          viewController: self
        )}() 
        
      let zelleFrame = CGRect(x:0, y:0, width:viewContainer.frame.width, height:viewContainer.frame.height) //desired location
      let zelleView = bridge.view(frame: zelleFrame) 

      // let zelleView = bridge.popup(anchor: self.view)
      view.addSubview(zelleView)
    }
} 
```

### 5. Create a new storyboard

Create a new storyboard and reference it with ZelleViewController.swift.

### 6. Create a new view

Create a view inside the storyboard and reference it with viewContainer.

### 7. Add permission in info.plist

Open the file info.plist (right-click > Open As > Source Code). Add the following keys to the file:

```json
<key>NSContactsUsageDescription</key>
<string>[PERMISSION_DESCRIPTION] </string>
<key>NSCameraUsageDescription</key>
<string>[PERMISSION_DESCRIPTION] </string>
<key>NSPhotoLibraryUsageDescription</key>
<string>[PERMISSION_DESCRIPTION] </string> 
```

Note: Minimum Xcode Version: 11 & Minimum OS: iOS 13

## Integrating Native code with React Native

### 1. Add Event Listener in app.js

Add the following content to the app.js file:

```json
import { NativeModules } from 'react-native';
import { NativeEventEmitter} from 'react-native'; 
        
const {MyModule} = NativeModules;
const myModduleEvent = new NativeEventEmitter(MyModule); 

//Add inside the class
componentDidMount(){
   this.subscription = myModduleEvent.addListener("getValue", event => {
     console.log("new event emitter2=====>",event.data); //Do your actions here
   });     

   this.subscription = myModduleEvent.addListener("sessionTimeout", event => {
      console.log("new event emitter2=====>",event.data); //Do your actions here
   });
 }  

 componentWillUnmount(){
    this.subscription.remove();
 } 
```

### 2. Calling native Android and iOS methods

Call the Android and iOS methods from the app.js file:

```json
var a = {};
var b = {};//here you can send added key value pair data to Zelle
b.param1 = "param1_value";
b.param2 = "param2_value";
b.param3 = "param3_value";

var c = {};//Prominent disclosure customized data for contact 
c.title = "We would like to access your phone contacts";
c.message = "We only sync phone numbers and email addresses from your contact list to help you add and pay a new recipient in Zelle®";

var d = {};//Prominent disclosure customized data for camera 
d.title = "We would like to access your camera";
d.message = "We only access your camera to help you add and pay a new recipient in Zelle®";

var e = {};//Prominent disclosure customized data for gallery 
e.title = "We would like to access your photos";
e.message = "We only access your photos to help you add and pay a new recipient in Zelle®";

a.pd_contact = c;
a.pd_camera = d;
a.pd_gallery = e;

var loaderData = {}; //Customized loader data 
loaderData.loaderColor = “hex color code”;
loaderData.bgColor = “hex color code”

var fi_callback = true

NativeModules.MyModule.NavigateToZelle("Demo Bank", "base_url", "institution_id", "product", "sso_key", fi_callback, loaderColor, b, a); 
```

### Sample Project:

[dps-zelle-sdk-testapp-reactnative.zip](https://github.com/Fiserv/zelle-turnkey-solutions/raw/develop/dps-zelle-sdk-testapp-reactnative.zip)


