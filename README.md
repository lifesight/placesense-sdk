RELEASE NOTES
February 14, 2018

PlaceSense SDK V1.7.4

Upgrade sensor APIs to collect location data, and user activity
Improved battery drain
Improved mobile data usage

ANDROID SDK INTEGRATION

Dependencies
 
These are the dependencies that are required in order to integrate the SDK. 
Retrofit 2.1.0 - This is used as HTTP Client to consume API calls
Play Service Location 11.6.0 - This is used for the device’s Activity Recognition and Location sensors.
Play Service GCM 11.6.0 - This is for using Job Schedulers to do network related operations in a backwards compatible manner, so that device that do not support Job Schedulers can also be able to do networking efficiently.

Installation

This section describes how to add Android SDK to your app.

Requirements
Android Studio or your favorite IDE
Android 4.0 or newer if Non-Beacon (Min SDK Version = API 14
Google Play Services 11.6.0 or newer


Installation

Add repository on build.gradle (app level)

Java
// add maven url
repositories {
    maven {
        url 'https://dl.bintray.com/veeruagrawal/maven/'
    }
}

Add ebizu publisher sdk into dependency on build.gradle (app level)

Java
//add dependency
compile 'com.ebizu.sdk.publisher:ebizupublishersdk:1.7.5'


Setup & Initialize

This is how to integrate with PlaceSense SDK:
Your Application ID should be included on Android Manifest, as follows

Java
<meta-data
     android:name="com.ebizu.APPLICATION_ID"
     android:value="<YOUR APPLICATION ID>" /> ### Initializing EbizuPublisher
  
Before you can use EbizuPublisher in your app, you must initialize it first.

Java
EbizuPublisher.getInstance().init(this, false, Config.PRODUCTION);
The initialization is done once, and you must provide an Android context, debug mode, and SDK environment. It is recommended to initialize EbizuPublisher at onCreate() on an application subclass:

Java

@Override
    public void onCreate() {
        super.onCreate();
        EbizuPublisher.getInstance().init(this, false, Config.PRODUCTION);
    }
    
    
Configure & Start Method

To start EbizuPublisher you must call start method. It is done as follows

Java

EbizuPublisher.getInstance().start(this);
EbizuPublisher start is done once, and you must provide an Android context before you initialize EbizuPublisher, and Location Permission should be granted in your app

Example :

Java

private void checkPermissionForLocation() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if ((ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) && (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED)) {
                requestPermissions(new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, LOCATION_PERMISSIONS_REQUEST);
            } else {
                EbizuPublisher.getInstance().start(this);
            }
        } else {
            EbizuPublisher.getInstance().start(this);
        }
    }
 
 @Override
     public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
         if (requestCode == LOCATION_PERMISSIONS_REQUEST) {
             if (grantResults.length == 1 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                 EbizuPublisher.getInstance().start(this);
             } else {
                 Toast.makeText(MainActivity.this, "Location permission denied", Toast.LENGTH_SHORT).show();
             }
         }
     }
     

INTEGRATING IOS SDK

Installation

Using Cocoapods

1.1 Set up CocoaPods on your system if you don't have it already. CocoaPods 1.1.0+ is required. 1.1

Make sure you have version 1.1.0 or newer by running this code from the terminal.

Shell
$ pod --version
1.2 Run the following to upgrade

Shell
$ sudo gem install cocoapods
1.3 Make sure your current Xcode project is closed.
1.4 Run the following set of commands in the terminal from your project's root directory.
Shell
pod init
echo "pod 'EbizuPublisherSuperLite'" >> Podfile
pod repo update
pod install
1.5 Open the newly created .xcworkspace file. (Make sure to always open the workspace from now
on)
Adding the SDK Manually
1.1 To install the SDK, download latest stable version of SDK archive.
Download PlaceSense SDK
1.2. Xcode with the iOS development kit is required to build an iOS app using PlaceSense SDK. For a better experience, we recommend using XCode 8 or later.
1.3. The SDK requires iOS 8.3 or later.
1.4. Open your project in Xcode.
1.5. Copy “EbizuPublisher.framework” to your project directory.
1.6. Make sure to Copy items into destination group's folder is selected.
1.7. Press the Finish button.
1.8. Ensure that you have added to your project the following dependent frameworks:
    - AdSupport.framework
     - CoreLocation.framework



Adding SDK Manually

Implementation

1.1 Initialize SDK Add following code to your AppDelegate.m
Objective-C
#import "AppDelegate.h"
#import <EbizuPublisher/EbizuPublisher.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    [[EbizuManager sharedManager] setDebugMode:YES];
    [EbizuManager initWithAppID:@"YOUR APP ID" withDevelopmentMode:NO];

    return YES;
}
1.2 Start location
Objective-C
#import <EbizuPublisher/EbizuPublisher.h>
...
    [EbizuManager start];
    
    
Questions?
We’re always happy to help with the code or other questions you might have about the platform! You can email us directly at developers@lifesight.io

