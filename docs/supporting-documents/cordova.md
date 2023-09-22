# Steps for Quick Start

## Project Setup

Prerequisites:

- Node JS
- Cordova
- Android SDK
- Xcode
- Android Emulator or Real Device
- iOS Simulator or Real Device

## Cordova Plugin for ZelleSDK(Android)

To integrate ZelleSDK into any Cordova projects, plugin files will be created that will act a bridge between native code and JavaScript.

- A plugin can be created using node.js for both Android and iOS platforms.

- To create a plugin, a plugin name, plugin ID, plugin version, and plugin package name are required. An example package name is com.zelle.sdk.plugin

- After a plugin is created, it contains some files inside the created plugin folder. Screenshot of the plugin folder:

![cordova_plugin_folder](../../assets/images/cordova_plugin_folder.jpg)

- Screenshot of the plugin files:

![cordova_plugin_files](../../assets/images/cordova_plugin_files.jpg)

- Inside the ZelleSDK Plugin folder, all folders and files are automatically generated except for the AAR folder. Create the AAR folder and move ZelleSDK.aar into it:

![cordova_plugin_aarfile](../../assets/images/cordova_plugin_aarfile.png)

- The src/android folder contains a java file that extends the Cordova plugin.

- The www folder contains a js file with same filename of the java file created in the src/android folder. This js and native file only communicate with each other for passing the parameters from js to java.

![cordova_android_folder](../../assets/images/cordova_android_folder.png)

- New java class and xml files can be created that will act as the interface to access ZelleSDK. In the plugin.xml file, the location path for the created files can be added in the android platform tag.

- After the plugin is created, it can be imported in any Cordova project to access ZelleSDK.

1. To integrate the plugin in the Cordova project, navigate to the Cordova project folder inside the terminal to add the plugin:

```
cordova plugin add "C:\Zelle SDK Plugin"
```

2.Build the Cordova project:

```
cordova build android
```

![cordova_plugin_added](../../assets/images/cordova_plugin_added.jpg)

- After the plugin is added, plugin files will be included in the Cordova project.

![cordova_project_strct](../../assets/images/cordova_project_strct.jpg)

3. Launch ZelleSDK from any Cordova application by calling ZellePlugin. Create the openZelle function in your .js file (index.js) and pass the required parameters to ZellePlugin:

```json
function openZelle(){
  var baseURL = document.getElementById("base_url");
  var institutionId = document.getElementById("institution_id");
  var ssoKey = document.getElementById("sso");
  var product = document.getElementById("product");
  var a = {};
var pdContact = {
title :"Contact Title",
message :"Contact Message",
};

var pdCamera = {
title :"Camera Title",
message :"Camera Message",
};

var pdPhoto = {
title :"Gallery Title",
message :"Gallery Message",
};

var appData = {
pd_contact :pdContact,
pd_camera  :pdCamera,
pd_gallery :pdPhoto,
};

var loaderData = {
loaderColor :"hex color code",
bgColor :"hex color code",
};

var zelleparam = {
param1 :"value1",
param2 :"value2",
param3 :"value3",
};


var success = function(message) { console.log(message); }
var failure =  function() { alert("Error calling Hello Plugin"); }
a.applicationName = "Demo Cordova App";
a.baseURL = baseURL.value;
a.institutionId = institutionId.value;
a.product = product.value;
a.ssoKey = ssoKey.value;
a.fiCallback = true;
a.loaderData = loaderData;
a.appData = appData;
a.parameters = zelleparam;
ZellePlugin.zelle_activity(person ,success,failure); //here we need to pass the data to Zelle plugin like this
}
```

## Cordova Plugin for ZelleSDK(iOS)

To integrate ZelleSDK into any Cordova projects, plugin files will be created that will act a bridge between native code and JavaScript.

- A plugin can be created using node.js for both Android and iOS platforms.

- To create a plugin, the plugin name, plugin ID, plugin version, and plugin package name are required. An example package name is com.zelle.sdk.plugin

After a plugin is created, it contains some files inside the created plugin folder. Screenshot of the plugin folder:

![cordova_ios_folder](../../assets/images/cordova_ios_folder.png)

- Inside the ZelleSDK Plugin folder, all folders and files are automatically generated except for the Framework folder. Create a Framework folder and place ZelleSDK.xcframework into it:

![cordova_ios_plugin](../../assets/images/cordova_ios_plugin.jpg)

- The src/iOS folder contains an Objective C/Swift file that extends the Cordova plugin.

- The www folder contains a js file with same filename of the Objective C/Swift file created in the src/iOS folder. This js and native file only communicate with each other for passing the parameters from js to Objective C/Swift.

- New Objective C/Swift and storyboard files can be created that will act as the interface to access ZelleSDK. In the plugin.xml file, the location path for the created files can be added in the iOS platform tag.

1. To integrate the plugin in the Cordova project, navigate to the Cordova project folder inside the terminal to add the plugin:

```
cordova plugin add "Users/Cordova/ZellePlugin"
```

2. Build the Cordova project:

```
cordova build ios
```

![cordova_ios_strct](../../assets/images/cordova_ios_strct.jpg)

3. Launch ZelleSDK from any Cordova application by calling ZellePlugin. Create the openZelle function in your .js file (index.js) and pass the required parameters to ZellePlugin.

```json
function openZelle(){
  var baseURL = document.getElementById("base_url");
  var institutionId = document.getElementById("institution_id");
  var ssoKey = document.getElementById("sso");
  var product = document.getElementById("product");
  var a = {};
var pdContact = {
title :"Contact Title",
message :"Contact Message",
};

var pdCamera = {
title :"Camera Title",
message :"Camera Message",
};

var pdPhoto = {
title :"Gallery Title",
message :"Gallery Message",
};

var appData = {
pd_contact :pdContact,
pd_camera  :pdCamera,
pd_gallery :pdPhoto,
};

var loaderData = {
loaderColor :"hex color code",
bgColor :"hex color code",
};

var zelleparam = {
param1 :"value1",
param2 :"value2",
param3 :"value3",
};

var success = function(message) {

console.log(message);
}
// session timeout callback
var failure =  function()
{ alert("Error calling Hello Plugin"); }

a.applicationName = "Demo Cordova App";
a.baseURL = baseURL.value;
a.institutionId = institutionId.value;
a.product = product.value;
a.ssoKey = ssoKey.value;
a.fiCallback = true;
a.loaderData = loaderData;
a.appData = appData;
a.parameters = zelleparam;

ZellePlugin.zelle_activity(person ,success,failure); //here we need to pass the data to Zelle plugin like this
}

```

### Kotlin Version Used in Sample Project

1.3.50

## Sample Project:

[dps-zelle-sdk-testapp-cordova-master.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11654405/dps-zelle-sdk-testapp-cordova-master.zip)
