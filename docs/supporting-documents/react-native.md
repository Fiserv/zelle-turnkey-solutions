# Steps for Quick Start

## Creating POC (Proof of Concepts) with React Native 

### Prerequisites:

Nodejs, react-native-cli or expo-cli, android emulator or real device, iOS simulator or real device 

### 1. Create new project (ReactDemo) using Terminal

```json
 npx create-expo-app ReactDemo
```

### 2. Move to project directory

```json
cd ReactDemo
```

### 3. Start the project

```json
npm start
```

Note: you can also use: npx expo start

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

Note: Now you will get android/ios folders in your project folder

### 6. Launch the application

```json
expo start
```

Note: This step ensures that the Metro bundler is running and serving your app's JS bundle for development. Leave this running and continue with the following steps

### 7. Running android 

Open the android directory in Android Studio, then build and run the project on an Android device or emulator

### 8. Running iOS project
