# Steps for Quick Start

## Project Setup

### 1. Prerequisites:

- Android studio (or) Visual Studio Code
- Flutter SDK
- Android Emulator (or) Real Device
- iOS Simulator (or) Real Device
- Android SDK
- Xcode    

### 2. Create new flutter application with both Android and iOS platform enabled. 

### 3. Select project type as Application and provide respective package name. 

### 4. Select specific language support for both Android and iOS and click finish to create the project.  

### 5. Once project has created navigate to lib folder and create dart class with respective widget to lauch Zelle.

### 6. To create a bridge between native and dart, First construct the channel. Use a Method channel with a single platform method that returns the data from dart to native and wise versa.

### 7. The client and host sides of a channel are connected through a channel name passed in the channel constructor. 

### 8. All channel names used in single app must be unique; prefix the channel name with a unique ‘domain prefix’. 
- Example: static const platform = MethodChannel(‘zellesdk.launch’);

```json
class _MyHomePageState extends State<MyHomePage> {
  static const platform = MethodChannel('zellesdk.launch');
```

### 9. Next, invoke a method on the method channel, specifying the concrete method to call using the String identifier launchZelle. 

### 10. Pass the respective parameter to lauch zellesdk in key, value pair in invokeMethod.

```json
// Loader Data Initialization:
Map<String, String> loaderData = new HashMap();
map['loaderColor'] = "hex color code";
map['bgColor'] = "hex color code";

// AppData Initialization:
Map<String,Map<String, String>> pdData = new LinkedHashMap();
//contact
Map<String, String> contact_pd = new LinkedHashMap();
contact_pd['title'] = 'CONTACT TITLE';
contact_pd['message'] = 'CONTACT MESSAGE';

//camera
Map<String, String> camera_pd = new LinkedHashMap();
camera_pd['title'] = 'CAMERA TITLE';
camera_pd['message'] = 'CAMERA MESSAGE';

//gallery
Map<String, String> gallery_pd = new LinkedHashMap();
gallery_pd['title'] = 'GALLERY TITLE';
gallery_pd['message'] = 'GALLERY MESSAGE';

pdData['pd_contact'] = contact_pd;
pdData['pd_camera'] = camera_pd;
pdData['pd_gallery'] = gallery_pd;

//Parameter Initialization: 
Map<String, String> map = new HashMap();
map['param1'] = "";
map['param2'] = "";
map['param3'] = "";

//Zelle Constructor Initialization:
final String result = await platform.invokeMethod('launchZelle',{
'applicationName': _applicationNameController.text,
'baseUrl':_baseUrlController.text,
"institutionId":_institutionIdController.text,
'product':_productController.text,
'ssoKey': _ssoKeyController.text,
'fi_callback': true,
'loaderData': loaderData, //optional
'appData': pdData,   //Optional
'parameter':map}); //optional
```

### 11. Use the returned result to update the user interface state.

## Android Platform:

### 12. Open Android platform inside android studio and create libs folder inside app.

### 13. Place the latest version of ZelleSDK.aar file inside the libs folder.

### 14. To add respective dependencies, open build.gradle file and inside dependencies add required dependency given below.

```json
implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
implementation 'androidx.appcompat:appcompat:1.4.2'
implementation 'com.google.android.material:material:1.4.0'
implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
implementation('com.journeyapps:zxing-android-embedded:4.3.0') { transitive = false}
implementation 'com.google.zxing:core:3.4.0'
implementation 'androidx.core:core-ktx:1.6.0'
implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
```

### 15. Open the file MainActivity.kt located in the kotlin folder in the Project view. Inside the configureFlutterEngine() method, create a MethodChannel and call setMethodCallHandler(). Make sure to use the same channel name as was used on the Flutter client side.

### 16. Create new activity to initialize and launch our ZelleSDK.

### 17. Get the arguments from the client side and pass those arguments to LaunchZelle.kt.

```json
MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
call, result ->
if (call.method == "launchZelle") {
genericTag = this
val hashMap = call.arguments as HashMap<*,*>
val  intent = Intent(this, LaunchZelle::class.java)
intent.putExtra("data", hashMap)startActivity(intent)
}
```

### 18. Open activity_launch_zelle  xml file inside layout folder and create a framelayout to initialize zelle view.

### 19. Open LauchZelle.kt file inside onCreateView function initialize Zelle, Bridge and BridgeView (or) BridgePopup with respective parameters.

```json
class LaunchZelle : AppCompatActivity() {
   override fun onCreate(savedInstanceState: Bundle?) {
     super.onCreate(savedInstanceState)
     setContentView(R.layout.activity_launch_zelle)

     if (intent != null) {
         val data = intent.getSerializableExtra("data") as HashMap<*, *>
         val zelle = Zelle(
         applicationName = data.get("applicationName") as String?,
         baseURL = data.get("baseUrl") as String,
         institutionId = data.get("institutionId") as String,
         product = data.get("product") as String,
         ssoKey = data.get("ssoKey") as String,
         fi_callback = false,  //optional one
         appData = data.get("appData") as Map<String, Map<String, String>>?,
         loaderData = null,
         parameters = data.get("parameter") as Map<String, String>
        )
        val bridge = Bridge(this, zelle)
        zelle.preCacheContacts = true
    //To show Bridge View
        val view = bridge.view()
        supportFragmentManager.beginTransaction().replace(R.id.lay_view, view).commit()

    //To show Bridge Popup
        val popup = bridge.popup()
        popup.show(supportFragmentManager  , popup.tag)
    }
  }
}
```
### 20. Create ZelleTheme inside Styles (or) Theme.xml file.

```json
<style name="ZelleTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
<item name="android:windowBackground">?android:colorBackground</item>
</style>
```

### 21. Register the LaunchZelle.kt activity inside application AndroidManifest.xml file with respective theme.

```json
<application
android:name="${applicationName}"
android:icon="@mipmap/ic_launcher"
android:theme="@style/LaunchTheme"
android:label="zellefiapp">
        
<activity
android:name=".LaunchZelle"
android:theme="@style/ZelleTheme"
android:exported="false" />
```

#### Note:
- AppData parameter passed in Zelle is an optional one. It is used to show the customized alert passed from FI (only for android not for iOS).

- Default values:
  1) Contacts:
     “title” => “We would like to access your phone contacts”,
     “message” => “We only sync phone numbers and email address from your contact list to help you add and pay a new recipient in zelle®.
  2) Camera:
     “title” => “We would like to access your camera”,
     “message” => “We only access your camera to help you add and pay a new recipient in zelle®.
  3) Gallery:
     “title” => “We would like to access your photos”,
     “message” => “We only access your photos to help you add and pay a new recipient in zelle®.

- ZelleSDK launches from MinimumSDK version 24.
- After zelle session is closed. To get the session tag value to client side, initialize genericTag and implement GenericTag to get the sessionTag value from ZelleSDK.
- “loaderData” is an optional parameter to pass hex color codes for custome loader. Keys to be used “loaderColor” and ”bgColor”.

### 22. Inside sessionTag override method get the data from Zelle and send result back to client side.

```json
zelleResult = result as String;
if(zelleResult.isNotEmpty){
print('SessionTag-------------------${zelleResult}');
}
} on PlatformException catch (e) {
zelleResult = "Failed to get result: '${e.message}'.";
}
```

### 23. Inside getValue override method get the data from Zelle and send result back to client side.

## iOS Platform:

### 24. Open iOS platform inside Xcode and create new ViewController to launch Zelle.

### 25. Place the ZelleSDK XCframework file inside the Runner.

### 26. Add the ZelleSDK required permissions in Info.plist file.
- Example: 
- 1) Contact permission
- 2) Camera permission
- 3) Gallery Permission

### 27. Open the file AppDelegate.swift located under Runner > Runner in the Project navigator.

### 28. Override the application:didFinishLaunchingWithOptions: function and create a FlutterMethodChannel tied to the channel name zellesdk.launch.

```json
  override func application(
_ application: UIApplication,
didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
) -> Bool {
let controller : FlutterViewController =  FlutterViewController()

let navigationController = UINavigationController(rootViewController: controller)
self.window.rootViewController = navigationController
navigationController.isNavigationBarHidden = true

let zelleChannel = FlutterMethodChannel(name: "zellesdk.launch",
binaryMessenger: controller.binaryMessenger)
```

### 29. Get the arguments from the client side and pass that arguments to created ViewController inside FlutterMethodChannel in AppDelegate.

```json
let zelleChannel = FlutterMethodChannel(name: "zellesdk.launch",
binaryMessenger: controller.binaryMessenger)
zelleChannel.setMethodCallHandler({
(call: FlutterMethodCall, result: @escaping FlutterResult) -> Void in
if call.method == "launchZelle" {
let data = call.arguments
let mainVC:LaunchZelleViewController = LaunchZelleViewController()
let nextVC = UIStoryboard(name: "Main", bundle: nil).instantiateViewController(withIdentifier: "LaunchZelleViewController") as!
LaunchZelleViewController
nextVC.getData = data as? NSDictionary
nextVC.flutterResult = result
navigationController.pushViewController(nextVC, animated: true)

} else {
result(FlutterMethodNotImplemented)
return
}
})
```

### 30. Now open created ViewController, Inside viewDidLoad() function initialize Zelle, Bridge, and BridgeView (or) BridgePopup to launch Zelle.

```json
override func viewDidLoad() {
  super.viewDidLoad()
  Bridge.genericTag = self
  appName = getData?.value(forKey: "applicationName") as? String
  baseUrl = getData?.value(forKey: "baseUrl") as? String
  institutionId = getData?.value(forKey: "institutionId") as? String
  product = getData?.value(forKey: "product") as? String
  ssoKey = getData?.value(forKey: "ssoKey") as? String
  parameters = getData?.value(forKey: "parameter") as? NSDictionary

  let zelle = Zelle(
  applicationName : appName,
  baseURL: baseUrl ?? "",
  institutionId: institutionId ?? "",
  product: product ?? "",
  ssoKey: ssoKey ?? "",
  loaderData: nil,
  parameters: parameters  as! [String : String]
)

lazy var bridge: Bridge = {
Bridge(config: zelle,
viewController: self
)
}()

let zelleFrame = CGRect(x:0, y:0, width: view.frame.width, height: view.frame.height) //desired location
let zelleView = bridge.view(frame: zelleFrame)

//  let zelleView = bridge.popup(anchor: self.view)
view.addSubview(zelleView)
}
```

### 31. Pass the required variables to the Zelle Class which is passed from the client side.

#### Note:
- XCframework requires minimum version iOS - 13.
- “loaderData” is an optional parameter to pass hex color codes for custome loader. Keys to be used “loaderColor” and ”bgColor”.
- After zelle session is closed. To get the session tag value to client side initialize genericTag and implement GenericTagDelegate to get the tag value from ZelleSDK.

```json
import UIKit
import ZelleSDK
class LaunchZelleViewController: UIViewController, GenericTagDelegate {
func getValue(name: String) {
print("\(name)")
}

var getData:NSDictionary?
var flutterResult:FlutterResult?
var appName:String? = ""
var baseUrl:String? = ""
var institutionId:String? = ""
var product:String? = ""
var ssoKey:String? = ""
var parameters:NSDictionary? = NSDictionary()
var callBack:String? = ""

func sessionTag(name tag: String) {
flutterResult!("\(tag)")
}
```
- Inside sessionTag override method get the data from Zelle and send result back to client side.
- Before sending data back to client side initialize the FlutterResult variable created in ViewController from AppDelegate.

```json
let nextVC = UIStoryboard(name: "Main", bundle: nil).instantiateViewController(withIdentifier: "LaunchZelleViewController") as!
LaunchZelleViewController
nextVC.getData = data as? NSDictionary
nextVC.flutterResult = result
navigationController.pushViewController(nextVC, animated: true)
```

### Sample Project: