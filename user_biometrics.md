# Device Biometry with .NET MAUI

To use biometrics on your app, is going to need the [Plugin.Fingerprint](https://www.nuget.org/packages/Plugin.Fingerprint/).<br>
**IMPORTANT:** don't forget to click on *Show Preleases* and select the version 3.0.0 or above.

## 01. Setup:

On your *MainActivity.cs* file, add the following procedure:

````csharp
protected override void OnCreate(Bundle savedInstanceState {
    base.OnCreate(savedInstanceState);
    CrossFingerprint.SetCurrentActivityResolver(() => this);
})
````

### iOS:

Add this line into your *info.plist*.

````
<key>NSFaceIDUsageDescription</key>
    <string>MESSAGE</string>
````

Replace **MESSAGE** for another text - This is the text that appears for the user to approve the usage of Face/Touch ID.

### Android:

To use on Android, add both options into your *AndroidManifest.xml*.<br>
*USER_FINGERPRINT* is required for older Android versions and *USER_BIOMETRIC* is for the new ones.

````
<uses-permission android:name="android.permission.USE_BIOMETRIC" />
<uses-permission android:name="android.permission.USE_FINGERPRINT"/>
````

## 02. Code:

Before using any biometry on your program, first request if it's available to use:

````csharp
var isBiometricAvailable = await CrossFingerprint.Current.IsAvailableAsync();
//// This won't ask the user if he would like to use biometry
// Just check if the device has an biometry and HAS registered at least one finger/face.
````

Now, let's scan user's face:

````csharp
if (isBiometricAvailable){

    // This will ask user to give permission
    var authResult = await CrossFingerprint.Current.AuthenticateAsync("MESSAGE");

    if (authResult.Authenticated){
        // Biometry matches
    } else {
        // Biometry won't match
    }
}
````
