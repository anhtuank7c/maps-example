# Guide to integrate react-native-maps

Video guide available here: https://youtu.be/XTwpYtNTV48
[![Video guide](http://i.imgur.com/Zz18vQD.png)](https://youtu.be/XTwpYtNTV48)


## This example still work well on `React Native 0.46.4`, version `0.47.1` work well. [Latest release](https://github.com/airbnb/react-native-maps/releases/tag/v0.16.0)

## Step 01: Create new project (ignore if you have one existing) and 
```
react-native init MyMap
cd MyMap
yarn add react-native-maps
```

## Step 02: link package

```
react-native link react-native-maps
```

## Step 03: Setup iOS project (Make sure you have [Cocoapods](https://cocoapods.org/) installed on your Mac)

```
cd ios
pod init
```

Open `Podfile` in your vscode (atom, phpstorm etc...)

```
# Podfile content
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '8.0'

target 'MyMap' do
  pod 'Yoga', :path => '../node_modules/react-native/ReactCommon/yoga'
  pod 'React', path: '../node_modules/react-native', :subspecs => [
    'Core',
    'RCTActionSheet',
    'RCTAnimation',
    'RCTGeolocation',
    'RCTImage',
    'RCTLinkingIOS',
    'RCTNetwork',
    'RCTSettings',
    'RCTText',
    'RCTVibration',
    'RCTWebSocket',
    'BatchedBridge'
  ]
  pod 'GoogleMaps'
end
```

## Step 04: install Podfile

```
pod install
```

## Step 05: Open project

```
open MyMap.xcworkspace
open .
```

Your project will be open in Xcode, drag and drop both `AirGoogleMaps`, `AirMaps` dir in `MyMap/node_modules/react-native-maps/lib/ios` to your Xcode project.

## Step 06: Enable Google Map SDK for iOS and Google Map Android API

Go to [Google console](https://console.developers.google.com/apis/library), choose or create new project, go to library then enable 2 libraries:
- Google Map SDK for iOS
- Google Map Android API

Then create new `Credentials` key

## Step 07: Edit AppDelegate.m

```
...
#import "AppDelegate.h"

#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.h>
@import GoogleMaps; # INSERT THIS LINE

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  NSURL *jsCodeLocation;
  [GMSServices provideAPIKey:@"YOUR_KEY"]; # INSERT THIS LINE

  jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index.ios" fallbackResource:nil];
...
```

You're done setting up iOS project, you can now `react-native run-ios`

## Step 08: Paste these line to `android/app/build.gradle`

```
dependencies {
    ...
    // Paste these line
    compile(project(':react-native-maps')){
        exclude group: 'com.google.android.gms', module: 'play-services-base'
        exclude group: 'com.google.android.gms', module: 'play-services-maps'
    }
    compile 'com.google.android.gms:play-services-base:11.+'
    compile 'com.google.android.gms:play-services-maps:11.+'
}
```

## Step 09: Insert key to `android/app/src/AndroidManifest.xml`

```
<application>
    <!-- You will only need to add this meta-data tag, but make sure it's a child of application -->
    <meta-data
      android:name="com.google.android.geo.API_KEY"
      android:value="YOUR GOOGLE MAPS API KEY HERE"/>
</application>
```

# WE ARE GOOD TO GO.



# LICENSE

MIT License

Copyright (c) 2017 anhtuank7c

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
