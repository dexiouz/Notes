# Setting up firebase performance monitor (Second App)

The set up for this project is 
- "appcenter": "^3.0.3"
- "react-native": "0.62.2",
- "@react-native-firebase/analytics": "^11.2.0",
- "@react-native-firebase/app": "^11.2.0",
- "@react-native-firebase/crashlytics": "^11.2.0",
- "@react-native-firebase/messaging": "^11.2.0",
- Plus others...

I installed firebase performance 
```sh
npm i @react-native-firebase/perf
```
 I got this error:

```
...
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer @react-native-firebase/app@"12.1.0" from @react-native-firebase/perf@12.1.0
npm ERR! node_modules/@react-native-firebase/perf
npm ERR!   @react-native-firebase/perf@"*" from the root project
...
```

Technically, `@react-native-firebase/app` is at `v11.2.0` but `@react-native-firebase/perf` needs `@react-native-firebase/app` to be at `v12.1.0`. 

Because of posibility of breaking changes and to override the error, I used 
```sh
$ npm install @react-native-firebase/perf --legacy-peer-deps
```

this set the perf package at `12.1.0` as at the time of this writing.

I ran
```sh
cd ios && pod install
```
and got this error

```sh
 [!] The 'Pods-Nobs' target has libraries with conflicting names: libgoogleutilities.a.
```
Then I ran this one to fix it
```sh
pod update
```

Now resolved, I tried to build the project 

```sh
 yarn ios
```
and got build error. Then I built the project with xcode to see the error which I found to be
```sh
 <event2/event-config.h> not found
```

Careful searching stackoverflow and github issues showed it's a *Flipper* issue. I need to remove flipper 

To remove flipper, go to  `yourProject/ios/podfile`, comment out 
```sh
 # add_flipper_pods!
  # post_install do |installer|
  #  flipper_post_install(installer)
  #end
```
Also goto yourApp/ios/yourAppName/AppDelegate.m , look for and comment  out
```c
// #if DEBUG
// #import <FlipperKit/FlipperClient.h>
// #import <FlipperKitLayoutPlugin/FlipperKitLayoutPlugin.h>
// #import <FlipperKitUserDefaultsPlugin/FKUserDefaultsPlugin.h>
// #import <FlipperKitNetworkPlugin/FlipperKitNetworkPlugin.h>
// #import <SKIOSNetworkPlugin/SKIOSNetworkAdapter.h>
// #import <FlipperKitReactPlugin/FlipperKitReactPlugin.h>
// static void InitializeFlipper(UIApplication *application) {
//   FlipperClient *client = [FlipperClient sharedClient];
//   SKDescriptorMapper *layoutDescriptorMapper = [[SKDescriptorMapper alloc] initWithDefaults];
//   [client addPlugin:[[FlipperKitLayoutPlugin alloc] initWithRootNode:application withDescriptorMapper:layoutDescriptorMapper]];
//   [client addPlugin:[[FKUserDefaultsPlugin alloc] initWithSuiteName:nil]];
//   [client addPlugin:[FlipperKitReactPlugin new]];
//   [client addPlugin:[[FlipperKitNetworkPlugin alloc] initWithNetworkAdapter:[SKIOSNetworkAdapter new]]];
//   [client start];
// }
// #endif
```
In same file look for and remove
```c
  // #if DEBUG
  //   InitializeFlipper(application);
  // #endif
```

then I ran 

```sh
pod update
```
And got another error 
```sh
<AppCenterCrashes/MSErrorReport.h> file not found
```

Careful searching showed it's appcenter issue. My appcenter packages are at `v3.0.3` but in podfile.lock they have latest versions `v4.1.01.

Here's what I did 
```
pod repo update
```
> Deleted `podfile.lock` and `pods folder` and rerun 

```sh
pod install
```
I also `Cleaned build folder in xcode - taskbar - Product - Clean Build folder`. 
Then I Built the project again via xcode and it still failed  again with this error 
```sh
line 2: node: command not found Using Node.js,
================================================
PhaseScriptExecution failed with a nonzero exit code
```
 Noticing that the first error has to do with node, I quickly built the project via terminal and boom!!!. It worked. Haha

So to fix the node issue. I installed node via nvm. because of that, xcode loses node path on run. I fixed it with 
```sh
ln -s $(which node) /usr/local/bin/node
```

 Ran the project via xcode again and ...  it worked finally. 

To continue performance set up for android, go to `/android/build.gradle` and add 
```sh
buildscript {
    dependencies {
        // ...
        classpath 'com.google.firebase:perf-plugin:1.4.0'
    }
```

Also go to the top of `/android/app/build.gradle` file and add 
```sh
apply plugin: 'com.google.firebase.firebase-perf'
```

Finally rebuild android
```sh
$ yarn android
```

All platforms working so gooood...