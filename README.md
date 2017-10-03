# Guide to integrate react-native-maps

## Step 01: Create new project (ignore if you have one existing)
```
react-native init MyMap
cd MyMap
yarn add react-native-maps
```

## Step 02: Setup iOS project (Make sure you have [Cocoapods](https://cocoapods.org/) installed on your Mac)

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
    
    pod 'GoogleMaps'  # <~~ remove this line if you do not want to support GoogleMaps on iOS
    pod 'react-native-maps', path: '../node_modules/react-native-maps/'
    pod 'react-native-google-maps', path: '../node_modules/react-native-maps/'  # <~~ if you need GoogleMaps support on iOS
end
post_install do |installer|
    installer.pods_project.targets.each do |target|
        if target.name == "react-native-google-maps"
            target.build_configurations.each do |config|
                config.build_settings['CLANG_ENABLE_MODULES'] = 'No'
            end
        end
    end
end
```

## Step 03: install Podfile

```
// If your pod repo too old, do update before install: pod repo update
pod install
```

## Step 04: Open project

```
open MyMap.xcworkspace
open .
```

- Drag this folder `node_modules/react-native-maps/lib/ios/AirGoogleMaps/` into your project, and choose `Create groups` in the popup window.
- In your project's `Build Settings` > `Header Search Paths`, double click the value field. In the popup, add `$(SRCROOT)/../node_modules/react-native-maps/lib/ios/AirMaps` and change `non-recursive` to `recursive`

## Step 05: Enable Google Map SDK for iOS and Google Map Android API (Very important)

Go to [Google console](https://console.developers.google.com/apis/library), choose or create new project, go to library then enable 2 libraries:
- Google Map SDK for iOS
- Google Map Android API

Then create new `Credentials` key

## Step 06: Edit AppDelegate.m

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

## Step 07: Paste these line to `android/app/build.gradle`

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

## Step 08: Insert key to `android/app/src/AndroidManifest.xml` (Very important)

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
