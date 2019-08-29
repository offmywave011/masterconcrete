## Scanbot SDK Dependencies

The Scanbot SDK for Android is distributed through our private Maven repository which you have specify in your **top-level** `build.gradle` file:

```groovy
allprojects {
    repositories {
        google()
        jcenter()

        // Scanbot SDK maven repos:
        maven {
            url 'https://nexus.scanbot.io/nexus/content/repositories/releases/'
        }
        maven {
            url 'https://nexus.scanbot.io/nexus/content/repositories/snapshots/'
        }
    }
}
```

Afterwards, you can add a dependency to your project:

For [Package I](https://scanbot.io/en/sdk.html#packages) features:

```groovy
implementation "io.scanbot:sdk-package-1:$scanbotSdkVersion"
```

Or alternative for [Package II](https://scanbot.io/en/sdk.html#packages) features:

```groovy
implementation "io.scanbot:sdk-package-2:$scanbotSdkVersion"
```

Get the latest `$scanbotSdkVersion` from [[Release History]].


## Enable Multidex
Make sure you have enabled [multidex](https://developer.android.com/studio/build/multidex) by setting `multiDexEnabled` to `true` in your **module-level** `build.gradle` file (e.g. `app/build.gradle`):

```groovy
android {
  ...
  defaultConfig {
    ...
    multiDexEnabled true
  }
}
```


## ABI Settings
The Scanbot SDK uses native libraries under the hood and supports following [ABIs](https://developer.android.com/ndk/guides/arch.html): `armeabi-v7a`, `arm64-v8a`, `x86` and `x86_64`.

Please check and add/adjust the `abiFilters` configuration in your **module-level** `build.gradle` file accordingly:

```groovy
android {
  ...
  defaultConfig {
    ...
    ndk {
      // a typical production configuration:
      abiFilters "armeabi-v7a", "arm64-v8a"
    }
  }
}
```

- `arm64-v8a`: Native libs for the `arm64-v8a` architecture are available since version **1.31.0** of Scanbot SDK. Previous versions of Scanbot SDK use `armeabi-v7a` libs since they are fully compatible with "arm64".

- `x86` and `x86_64`: In most cases both "x86" architectures can be removed for the release (production) build, since they are only used on emulators and on some rare devices with the Intel Atom architecture. The support for the `x86_64` architecture was added in **v1.50.0** of Scanbot SDK.

If you need to support **all** architectures, we highly recommend to use the new [Android App Bundle](https://developer.android.com/platform/technology/app-bundle/) approach when it comes to publishing apps.


## Tuning the Android Manifest

### Camera Permission

Declare that your app needs camera permission by listing the permission `android.permission.CAMERA` in the `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" />
```

Since Android 6.0 (API level 23) you also have to implement a suitable permission requesting and handling **at runtime**. For more details please see the Android docs ["Request App Permissions"](https://developer.android.com/training/permissions/requesting) as well as our basic [example implementation](https://github.com/doo/scanbot-sdk-example-android/blob/master/classical-components-demo/camera-view/src/main/java/io/scanbot/example/MainActivity.java).


### Memory Settings

Since your application will work with high-resolution images it is strongly recommended to add the property `android:largeHeap="true"` in the `<application>` element of your `AndroidManifest.xml` file, especially for Android <= 7.x. Processing hi-res images is a memory intensive task and this property will ensure your app has enough heap allocated to avoid `OutOfMemoryError` exceptions.

```xml
<application android:largeHeap="true" ...>
  ...
</application>
```

👉 We also recommend to check out this article [Managing Bitmap Memory](https://developer.android.com/topic/performance/graphics/manage-memory) of the Android docs.


## Initialize SDK

The Scanbot SDK must be initialized before usage. Add the following code snippets to your `Application` class:

```java
import io.scanbot.sdk.ScanbotSDKInitializer;

@Override
public void onCreate() {
    new ScanbotSDKInitializer()
      .license(this, LICENSE_KEY) // see below
      .initialize(this);

    super.onCreate();
}
```

Please make sure to use the `ScanbotSDKInitializer` class from the new Java package `io.scanbot.sdk.` (`net.doo.snap.ScanbotSDKInitializer` is deprecated).


## Add Scanbot SDK License Key

Use the method `license(application, "YOUR_SCANBOT_SDK_LICENSE_KEY")` on initialization of the Scanbot SDK to set your license key:

```java
// This is an expired license key. Used only for demo code purposes!
String LICENSE_KEY =
    "MiKudIf/IFGnwk/0ZQAhx58IGEV8/L" +
    "d6dF/F/j2TyLNNb+TCaIsQLOf2+/Ih" +
    "LRRPvRsHESA/b0KFqnxYl4briv3kzQ" +
    "tSTrvDJ9mE15rxU/Pob4XRnjTSc9B7" +
    "P/CpM0HDkmM02U8ljt0kKwpQSRZkiz" +
    "rBe+qttmA0b00ZEcyfn9t2xP4zRdaD" +
    "ayjWAdZi82oP3UST3bLIV8I90Qordf" +
    "s+u23JKL52bQY+HB1HNW7Ow8rA4+Mg" +
    "Lr/wE2N+UBElHPOZLtQBTHYUvbVwVU" +
    "KZDWOEv8VfrTmn2XA0tKVSDaxL6z7B" +
    "1s2g83XWYaxXrf4A3q/fkSNvDN1oic" +
    "Dvs0gbD1Lf0w==\nU2NhbmJvdFNESw" +
    "ppby5zY2FuYm90LmV4YW1wbGUuc2Rr" +
    "LmFuZHJvaWQKMTQ3ODA0NDc5OQoyCj" +
    "I=\n";

new ScanbotSDKInitializer()
        .license(this, LICENSE_KEY)
        .initialize(this);
```

Please make sure that you have inserted the exact key as it is stated in the license file - with all encoded line breaks `\n`.


## License Checks in Production Apps

⚠️ If your Scanbot SDK license has expired, any call of the Scanbot SDK API will terminate your app. To prevent this you should always check for license expiration during the runtime by calling the method `scanbotSDK.isLicenseValid()`. If this method returns `false`, you should disable any usage of the Scanbot SDK (functions, UI components, etc).

**We highly recommend to implement a suitable handling of this case in your app!**

Example code for checking the license status:

```java
import io.scanbot.sdk.ScanbotSDK;

ScanbotSDK scanbotSDK = new ScanbotSDK(activity);
if (scanbotSDK.isLicenseValid()) {
    // Making your call into ScanbotSDK API is safe now.
    // e.g. startDocumentScanner() or doSomeScanbotImageOperation()
}
```
