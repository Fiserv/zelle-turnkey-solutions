# Steps for Quick Start

## Project Setup

### 1. Prerequisites:

- Visual studio
- Xamarin
- Android SDK
- Xcode
- Android Emulator (or) Real Device
- iOS Simulator (or) Real Device

### 2. Create Interface Class

Create a IZelleService.cs interface class inside Xamarin Solution.

This IZelleService.cs class will create a bridge between Xamarin Solution to Xamarin Android and iOS.

### 3. Code Snippet for IZelleService Interface class:

```json
using System;
using System.Collections;

namespace FIApp
{
    public interface IZelleService
  {
      void launchZell(String applicationName, String baseUrl, String institutionId, String ssoKey, String product, IDictionary data);
  }
}
```

### 4. Launch Zelle using DependencyService

In Xamarin solution launch the Zelle by passing the required parameters through interface by using DependencyService.

```json
using System;
using Xamarin.Forms;
        
namespace FIApp
{
    public partial class MainPage : ContentPage
  {
    public MainPage()
  {
  InitializeComponent();

}

public void Button_Clicked(object sender, EventArgs args)
  {
    var callZelle = DependencyService.Get<IZelleService>();
    if (callZelle != null)
      {

      callZelle.launchZell(ApplicationName.Text, BaseUrl.Text, InstitutionId.Text, SSOKey.Text, ProductId.Text, null);

     }
    }
  }
}
```

## a) ZelleSDK Integration for Xamarin(Android)

### 1. Import Library

Add the required library’s from NuGet for Xamarin android inside <AppName.android>.Packages.

The library’s which is shown in the below image packages folder is required to launch ZelleSDK.

### 2. Add Required Permission

Add the required permission and declare the respective class file path and tools:node="replace" inside application tag in Properties.AndroidManifest.xml file.

```json
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:versionCode="1"
android:versionName="1.0"
package="com.fiserv.dps.mobile.fiapp">

<uses-sdk android:minSdkVersion="21" android:targetSdkVersion="30" />

<application android:label="FIApp.Android"
tools:node="replace"
android:theme="@style/MainTheme">
<activity android:name="com.fiserv.dps.mobile.sdk.activity.ScanQRActivity" />
<activity android:name="com.fiserv.dps.mobile.sdk.activity.ContactDetailActivity" />
</application>

<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
</manifest>
``` 

### 3. Import DLL File

Add the ZelleSDKAndroidBinding and ZxingEmbedded .dll file inside References folder.

Open the MainActivity.cs file and implement the interface class inside <ProjectName.android> folder.

Add the assembly for the interface communication connection from Xamarin solution to Xamarin android and ios.

This IZelleService.cs interface class will override launchZell function.

```json
using Android.App;
using Android.Content.PM;
using Android.Runtime;
using Android.OS;
using Android.Content;
using FIApp.Droid;
using Xamarin.Forms;
using System.Collections;

[assembly: Dependency(typeof(MainActivity))]
namespace FIApp.Droid
{
[Activity(Label = "FIApp", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity, IZelleService
{
public static Xamarin.Forms.Platform.Android.FormsAppCompatActivity Instance { get; private set; }

protected override void OnCreate(Bundle savedInstanceState)
{
base.OnCreate(savedInstanceState);
Xamarin.Essentials.Platform.Init(this, savedInstanceState);
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
LoadApplication(new App());
Instance = this;

}
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
{
Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}

public void launchZell(string applicationName, string baseUrl, string institutionId, string ssoKey, string product, IDictionary parameters)
{
//Intent intent = new Intent(Forms.Context, typeof(ZelleActivity));
//Forms.Context.StartActivity(intent);

Intent intent = new Intent(MainActivity.Instance, typeof(ZelleActivity));
intent.PutExtra("applicationName", applicationName);
intent.PutExtra("baseUrl", baseUrl);
intent.PutExtra("institutionId", institutionId);
intent.PutExtra("ssoKey", ssoKey);
intent.PutExtra("product", product);
MainActivity.Instance.StartActivity(intent);
}
}
}
``` 

Get the values from launchZelle function and by using intent pass the values from one page to another.

```json
public void launchZell(string applicationName, string baseUrl, string institutionId, string ssoKey, string product, IDictionary parameters)
{
//Intent intent = new Intent(Forms.Context, typeof(ZelleActivity));
//Forms.Context.StartActivity(intent);

Intent intent = new Intent(MainActivity.Instance, typeof(ZelleActivity));
intent.PutExtra("applicationName", applicationName);
intent.PutExtra("baseUrl", baseUrl);
intent.PutExtra("institutionId", institutionId);
intent.PutExtra("ssoKey", ssoKey);
intent.PutExtra("product", product);
MainActivity.Instance.StartActivity(intent);   }
``` 

### 4. Create a Framelayout

Create XML file inside layout folder to design the framelayout to access Zelle.

```json
<?xml version="1.0" encoding="UTF-8" ?>
<LinearLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
android:layout_width="match_parent"
android:layout_height="match_parent">

<FrameLayout
android:id="@+id/lay_view"
android:layout_width="match_parent"
android:layout_height="match_parent" />
</LinearLayout>
``` 

### 5. Create a ZelleLaunch Class 

Create a new class which extends AppCompatActivity and override OnCreate function.

Set the View for that class inside OnCreate function:
- SetContentView(Resource.Layout.zelle_activity);

### 6. Initialize Zelle

Get the values from the previous activity using intent and initialize Zelle with respective params.

```json
Code Snippet of  Zelle Class:

using Android.App;
using Android.OS;
using AndroidX.AppCompat.App;
using Com.Fiserv.Dps.Mobile.Sdk.Bridge.Zelleview;
using Com.Fiserv.Dps.Mobile.Sdk.Bridge.Model;
using Com.Fiserv.Dps.Mobile.Sdk.Interfaces;
using Xamarin.Forms;
using Android.Widget;
using System.Collections;

namespace FIApp.Droid
{
[Activity(Label = "ZelleActivity", Theme = "@style/MainTheme", MainLauncher = true)]
public class ZelleActivity:AppCompatActivity, IGenericTag
{
public void SessionTag(string tag)
{
Toast.MakeText(Forms.Context, tag, ToastLength.Long).Show();
}

public void getValue  (string value)
{
Toast.MakeText(Forms.Context, value, ToastLength.Long).Show();
}


protected override void OnCreate(Bundle savedInstanceState)
{
base.OnCreate(savedInstanceState);
Xamarin.Essentials.Platform.Init(this, savedInstanceState);
// Set our view from the "main" layout resource
SetContentView(Resource.Layout.zelle_activity);

BridgeView.GenericTag = this;

//getting data from MainActivity
string applicationName = Intent.GetStringExtra("applicationName") ?? "Data not available";
string baseUrl = Intent.GetStringExtra("baseUrl") ?? "Data not available";
string institutionId = Intent.GetStringExtra("institutionId") ?? "Data not available";
string ssoKey = Intent.GetStringExtra("ssoKey") ?? "Data not available";
string product = Intent.GetStringExtra("product") ?? "Data not available";

Zelle zelle = new Zelle(
applicationName,
baseUrl,
institutionId,
product,
ssoKey,
true, // fi_callback,
appData, // Optional
null
);

Bridge bridge = new Bridge(this, zelle);
zelle.PreCacheContacts = true;
BridgePopup bridgeView = bridge.Popup();

//To show the zelle view in popup
// bridgeView.Show(SupportFragmentManager, bridgeView.Tag);


//To show Zelle view
SupportFragmentManager // Get the support fragment manager instead of the FragmentManager
.BeginTransaction() // Start a transaction
.Replace(Resource.Id.lay_view, bridgeView) // Add the fragment
.Commit(); // Commit the changes
}

}
}
``` 

### 6. Launch Zelle

Run the application to launch the Zelle.

## b) ZelleSDK Integration for Xamarin(iOS)

### 1. There are 2 approaches to integrate the swift library with Xamarin Application Project.

- * By updating the Swift source code to generate the header and mark the required members with @objc attribute

- * By creating a proxy framework where you control the public interface and proxy all the calls to underlying framework.  

We have followed the second approach where we are creating Proxy Framework which embeds the ZelleSDK framework.

### 2. Steps to create the Proxy Framework Project is as follows.

Open Xcode and create new Swift framework, which will be a proxy between Xamarin.iOS code and ZelleSDK Swift framework. Click File > New > Project and follow the wizard steps:

Select the SwiftFrameworkProxy from the project files explorer then select the General tab.

### 3. Import ZelleSDK.xcframework:

Drag and drop the ZelleSDK.xcframework package to the Xcode Frameworks and Libraries list under the General tab check the Copy items if needed option while adding the framework.

Verify that the Swift framework has been added to the project otherwise the following options will not be available.

Ensure that the Do Not Embed option is selected, which will be later controlled manually.

Ensure that the Build Settings option Always Embed Swift Standard Libraries, which includes Swift libraries with the framework is set to No. It will be later manually controlled, which Swift dylibs are included into the final package.

Ensure that the Enable Bitcode option is set to No. As of right now Xamarin.iOS doesn't include Bitcode while Apple requires all libraries to support the same architectures.

You can verify that the resulted framework has the Bitcode option disabled by running the following terminal command against the framework:

- otool -l SwiftFrameworkProxy.framework/SwiftFrameworkProxy | grep __LLVM

The output should be empty otherwise you want to review the project settings for your specific configuration.

### 4. Specifies a Header Name

Ensure that the Objective-C Generated interface Header Name option is enabled and specifies a header name. The default name is <FrameworkName>-Swift.h:

Expose desired methods and mark them with @objc attribute and apply additional rules defined below. If you build the framework without this step, the generated Objective-C header will be empty and Xamarin.iOS won't be able to access the Swift framework members. Expose the initialization logic for the underlying Gigya Swift SDK by creating a new Swift file ZelleProxy.swift and defining the following code:

### 5. Set Release Build

Change the scheme build configuration from Debug to Release. In order to do that, open the Xcode > Target > Edit Scheme dialog and then set the Build Configuration option to Release:

At this point, the Framework is ready to be created. Build the framework for both simulator and device architectures and then combine the outputs as a single framework package. Identify installed SDK versions in order to build the source code using xcodebuild tool:

- xcodebuild -showsdks

### 6. The output will be similar to the following:

```json
iOS SDKs:
iOS 13.2                        -sdk iphoneos13.2
iOS Simulator SDKs:
Simulator - iOS 13.2            -sdk iphonesimulator13.2
macOS SDKs:
DriverKit 19.0                  -sdk driverkit.macosx19.0
macOS 10.15                     -sdk macosx10.15
tvOS SDKs:
tvOS 13.2                       -sdk appletvos13.2
tvOS Simulator SDKs:
Simulator - tvOS 13.2           -sdk appletvsimulator13.2
watchOS SDKs:
watchOS 6.1                     -sdk watchos6.1
watchOS Simulator SDKs:
Simulator - watchOS 6.1         -sdk watchsimulator6.1
```

### 7. Generate framework file for iphoneOS and iphoneSimulator

Pick a desired iOS SDK and iOS Simulator SDK version, in this case version 13.2 and execute the build with the following command

- xcodebuild -sdk iphonesimulator13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
- xcodebuild -sdk iphoneos13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release

### 8. Create a fat framework

There are two Swift frameworks, one for each platform, combine them as a single package to be embedded into a Xamarin.iOS binding project later. In order to create a fat framework, which combines both architectures, you need to do the following steps. The framework package is just a folder so you can do all types of operations, such as adding, removing, and replacing files:

Navigate to the build output folder with Release-iphoneos and Release-iphonesimulator subfolders and copy one of the frameworks as an initial version of the final output (fat framework).

- cp -R "Release-iphoneos" "Release-fat"

Combine modules from another build with the fat framework modules

- cp -R "Release-iphonesimulator/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/" "Release-fat/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/"

### 9. Combine iphoneos + iphonesimulator configuration as a fat framework

- lipo -create -output "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphoneos/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphonesimulator/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"

### 10. Verify results

- lipo -info "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"

The output should render the following, reflecting the name of the framework and included architectures:

- Architectures in the fat file: Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy are: x86_64 arm64
- Note:  If you want to support just a single platform (for example, you are building an app, which can be run on a device only) you can skip the step to create the fat library and use the output framework from the device build earlier.

### 11. Prepare metadata

At this time, you should have the framework with the Objective-C generated interface header ready to be consumed by a Xamarin.iOS binding. The next step is to prepare the API definition interfaces, which are used by a binding project to generate C# classes. These definitions could be created manually or automatically by the Objective Sharpie tool and the generated header file. Use Sharpie to generate the metadata:

### 12. Objective Sharpie

Download the latest Objective Sharpie tool from the official downloads website and install it by following the wizard. Once the installation is completed, you can verify it by running the sharpie command:

- sharpie -v

Generate metadata using sharpie and the autogenerated Objective-C header file:

- sharpie bind --sdk=iphoneos13.2 --output="XamarinApiDef" --namespace="Binding" --scope="Release-fat/SwiftFrameworkProxy.framework/Headers/" "Release-fat/SwiftFrameworkProxy.framework/Headers/SwiftFrameworkProxy-Swift.h"

The output reflects the metadata files being generated: ApiDefinitions.cs and StructsAndEnums.cs. Save these files for the next step to include them into a Xamarin.iOS binding project along with the native references:

```json
Parsing 1 header files...
Binding...
[write] ApiDefinitions.cs
[write] StructsAndEnums.cs
```

The tool will generate C# metadata for each exposed Objective-C member, which will look similar to the following code. As you can see it could be defined manually because it has a human-readable format and straightforward members mapping:

```json
[Export ("launchZelleWithViewController:applicationName:baseUrl:institutionId:ssoKey:product:")]
void LaunchZelleWithViewController (UIViewController viewController, string applicationName, string baseUrl, string institutionId, string ssoKey, string product);
```

### 13. Build a binding library

The next step is to create a Xamarin.iOS binding project using the Visual Studio binding template, add required metadata, native references and then build the project to produce a consumable library:

Open Visual Studio for Mac and create a new Xamarin.iOS binding library project, give it a name, in this case SwiftFrameworkProxy.Binding and complete the wizard. The Xamarin.iOS binding template is located by the following path: iOS > Library > Binding Library:

### 14. Add ApiDefinition.cs and Structs.cs file

Delete existing metadata files ApiDefinition.cs and Structs.cs as they will be replaced completely with the metadata generated by the Objective Sharpie tool.

Copy metadata generated by Sharpie at one of the previous steps, select the following Build Action in the properties window: ObjBindingApiDefinition for the ApiDefinitions.cs file and ObjBindingCoreSource for the StructsAndEnums.cs file:

The metadata itself describes each exposed Objective-C class and member using C# language. You are able to see the original Objective-C header definition alongside with the C# declaration:

```json
// @interface SwiftFrameworkProxy : NSObject
[BaseType (typeof(NSObject))]
interface SwiftFrameworkProxy
{
// -(Void)launchZelleWithViewController: (UIViewController *) viewController : (NSString * _Nonnull) applicationName  :(NSString * _Nonnull) baseUrl : (NSString * _Nonnull) institutionId: (NSString * _Nonnull) ssoKey :(NSString * _Nonnull) product __attribute__((objc_method_family("none"))) _attribute__((warn_unused_result));
[Export ("launchZelleWithViewController:")]
LaunchZelleWithViewController (UIViewController viewController, string applicationName, string baseUrl, string institutionId, string ssoKey, string product);

}
```

Even though it's a valid C# code, it's not used as is but instead is used by Xamarin.iOS tools to generate C# classes based on this metadata definition. As a result, instead of the interface SwiftFrameworkProxy you get a C# class with the same name, which can be instantiated by your Xamarin.iOS code. This class gets methods, properties, and other members defined by your metadata, which you will call in a C# manner.

### 15. Add Native Reference

Add native reference to the generated earlier fat framework, as well as each dependency of that framework. In this case, add both SwiftFrameworkProxy and ZelleSDK framework native references to the binding project:

- To add native framework references, open finder and navigate to the folder with the frameworks. Drag and drop the frameworks under the Native References location in the Solution Explorer. Alternatively, you can use the context menu option on the Native References folder and click Add Native Reference to look up the frameworks and add them:

- Update properties of every native reference and check three important options:
  •	Set Smart Link = true
  •	Set Force Load = false
  •	Set list of Frameworks used to create the original frameworks. In this case each framework has only two dependencies: Foundation and UIKit. Set it to the Frameworks field:

### 16. Consume the binding library

The final step is to consume the Xamarin.iOS binding library in a Xamarin.iOS application. Create a new Xamarin.iOS project, add reference to the binding library, and activate Swift SDK:

- Create Xamarin.iOS project. You can use the iOS > App > Single View App as a starting point:

- Add a binding project reference to the target project or .dll created previously. Treat the binding library as a regular Xamarin.iOS library:

- Update the source code of the app and add the initialization logic to the primary SampleViewController, which activates ZelleSDK.

```json
public override void ViewDidLoad()
{
  base.ViewDidLoad();
  var zelle = new ZelleProxy();
  zelle.LaunchZelleWithViewController (this, “Demo Xamarin App”, "https://rxp-ui-test1.checkfreeweb.com/ftk/", “homeId”,  “key generated from payload url”, “zelle”);
}
```

### 17. Launch Zelle

Run the application to launch the Zelle.




