# Steps for Quick Start

## Integrating Zelle Plugin with Cordova Project

- Navigate to respective Cordova project folder using terminal to add the [ZellePlugin](?path=docs/supporting-documents/CordovaPluginFiles/ZellePlugin_V_1.0.1.md).
- Use the command to add plugin to the project —> cordova plugin add "C:\Zelle SDK Plugin"
- Create the function named openZelle in your .js file (e.g. index.js) and pass the required parameters to ZellePlugin.
```json
function openZelle(){
var baseURL = document.getElementById("base_url");
var institutionId = document.getElementById("institution_id");
var ssoKey = document.getElementById("sso");
var product = document.getElementById("product");
var a = {};
var b = {};
var pd_contact= {
pd.tittle : ”tittle” ,
pd.message : ”test”
};

b.param1 = "value1";
b.param2 = "value2";
b.param3 = "value3";


var success = function(message) { console.log(message); }
var failure =  function() { alert("Error calling Hello Plugin"); }
a.applicationName = "Demo Cordova App";
a.baseURL = baseURL.value;
a.institutionId = institutionId.value;
a.product = product.value;
a.ssoKey = ssoKey.value;
a.parameters = b;
a. appData = pd.contact; // only pass contact
ZellePlugin.zelle_activity(person ,success,failure); //here we need to pass the data to zelle plugin like this
}
```

- Build the project like —> cordova build “platform-name”

Note: For example cordova build android/cordova build ios

## Additional Information

## Cordova Plugin for ZelleSDK

To integrate ZelleSDK into any Cordova projects, plugin files will be created, which will act as a bridge between native code and JavaScript. Plugins can be created using Node.js for both Android and iOS platforms. To create a plugin, the plugin name, plugin id, plugin version, and plugin package name are required. (Example package name: com.zelle.sdk.plugin)


### Prerequisites:

- Node JS
- Cordova
- Android SDK
- Xcode
- Android Emulator (or) Real Device
- iOS Simulator (or) Real Device

### 1. Node and npm installation

- Install Node.js. —> https://nodejs.org/en/download/ (If not installed already)
- Open terminal and install —> npm install -g plugman (or) sudo npm install -g plugman

### 2. Create Cordova Plugin

- Create cordova plugin using plugman like —> plugman create --name PluginName --plugin_id com.example.sample.plugin --plugin_version 0.0.1

### 3. Adding android and iOS platform

- Navigate to project folder —>cd PluginName/src
- Add android platform to plugin ——> plugman platform add --platform_name android
- Add iOS platform to plugin ——> plugman platform add --platform_name ios

### 4. Create package.json File

- Navigate back to plugin folder ——>cd .. PluginName
- To create package.json file use the following commands ——>plugman createpackagejson "path of your PluginName" (or) sudo plugman

### 6. Screenshot of plugin: - This refers to the plugin folder. 

### 7. Integrating Android ZelleSDK with Cordova Plugin

- Create AAR folder inside the plugin project folder
- Then add android SDK (ZelleSDK.aar) into the AAR folder
- Open ZellePlugin/src/android folder in Android Studio
- It will contain ZellePlugin.java and android.iml, now we have to create the below files in android folder
  - zelleGradle.gradle
  - ZelleActivity.java
  - activity_zelle.xml
- Passing data between plugin and .js file using ZellePlugin.java
```json
import org.apache.cordova.CordovaPlugin;
import org.apache.cordova.CallbackContext;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import android.content.Context;
import android.content.Intent;
import android.widget.Toast;
com.fiserv.dps.mobile.sdk.interfaces.GenericTag;
/**
 * This class echoes a string called from JavaScript.
 */
//implement GenericTag to get session timeout details from SDK        
public class ZellePlugin extends CordovaPlugin  implements GenericTag {
	
	CallbackContext callbackContext1;

    @Override
    public boolean execute(String action, JSONArray args, CallbackContext callbackContext) throws JSONException {
        Context context = cordova.getActivity().getApplicationContext();
		BridgeView.genericTag= this;
		callbackContext1=callbackContext; 
        
        //Get the SDK's required details from cordova project(.js file)
                if(action.equals("zelle_activity")) {
                    String  message;
                    String duration;
                    try {
                        JSONObject options = args.getJSONObject(0);
                        Intent intent = new Intent(context, ZelleActivity.class);
                        intent.putExtra("ZelleObject", options.toString());
                        this.cordova.getActivity().startActivity(intent);//passing the data to ZelleActivity.java
                    } catch (JSONException e) {
                        callbackContext.error("Error encountered: " + e.getMessage());
                        return false;
                    }
                    return true;
                }
                return false;
    }

    private void coolMethod(String message, CallbackContext callbackContext) {
        if (message != null && message.length() > 0) {
            //sesstion timeout details passing to the cordova project(.js file)
            callbackContext.success(message);
        } else {
            callbackContext.error("Expected one non-empty string argument.");
        }
    }
    //Handling session timeout        
	@Override public void sessionTag(@NonNull String s) { this.coolMethod(s, callbackContext1); } }
}

```
        
- Launching Zelle inside ZelleActivity.java class
```json
public class ZelleActivity extends AppCompatActivity {
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        String package_name = getApplication().getPackageName();
        setContentView(getApplication().getResources().getIdentifier("activity_zelle", "layout", package_name));
        Intent intent = getIntent();
        try {
            if (intent != null){
            JSONObject jsonObject = new JSONObject(intent.getStringExtra("ZelleObject"));
            HashMap<String, String> param = new Gson().fromJson(jsonObject.getString("parameters"), HashMap.class);
            Zelle zelle = new Zelle(
                    jsonObject.getString("applicationName"),
                    jsonObject.getString("baseURL"),
                    jsonObject.getString("institutionId"),
                    jsonObject.getString("product"),
                    jsonObject.getString("ssoKey"),
                    param
            );
            Bridge bridge = new Bridge(ZelleActivity.this, zelle);
            zelle.setPreCacheContacts(true);
            BridgeView view1 = bridge.view();
            getSupportFragmentManager().beginTransaction().replace(R.id.frame_layout, view1).commit();
          }
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
}
```

- Creating layout inside activity_zelle.xml
```json
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    tools:context="com.fiserv.dps.mobile.zelleplugin.ZelleActivity">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Zelle Activity"
        android:id="@+id/textView"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

- Adding dependencies inside zelleGradle.gradle
```json
repositories {
    maven { url "https://jitpack.io" }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
    implementation "com.google.code.gson:gson:2.8.6"
    implementation('com.journeyapps:zxing-android-embedded:4.2.0') { transitive = false }
    implementation "com.google.zxing:core:3.4.0"
    implementation "androidx.appcompat:appcompat:1.3.1"
    implementation "androidx.constraintlayout:constraintlayout:2.1.1"
    implementation "com.google.android.material:material:1.4.0"
    implementation "androidx.core:core-ktx:1.5.0"
}
```

### Sample Project:

[dps-zelle-sdk-testapp-cordova-master.zip](https://github.com/Fiserv/zelle-turnkey-solutions/files/11654405/dps-zelle-sdk-testapp-cordova-master.zip)
