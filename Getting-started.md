All of the following steps are assuming that you are using Gradle as a build tool. It is also possible to use it with Maven, but you should adjust the steps accordingly.

## Scanbot SDK Dependencies

The Scanbot SDK for Android is distributed through our private repository which you should specify in your `build.gradle`:

    allprojects {
        repositories {
            jcenter()

            maven {
                url 'https://nexus.scanbot.io/nexus/content/repositories/releases/'
            }
            maven {
                url 'https://nexus.scanbot.io/nexus/content/repositories/snapshots/'
            }
        }
    }

Afterwards, you can add a dependency to your project:

For [Package I](https://scanbot.io/en/sdk.html#packages) features:

    compile "io.scanbot:sdk-package-1:$latestVersion"

Or alternative for [Package II](https://scanbot.io/en/sdk.html#packages) features:

    compile "io.scanbot:sdk-package-2:$latestVersion"

Get the `$latestVersion` number from the [README](https://github.com/doo/Scanbot-SDK-Examples/blob/master/README.md)

## ABI settings
The Scanbot SDK uses native libraries under the hood and supports following [ABIs](https://developer.android.com/ndk/guides/arch.html): `armeabi-v7a`, `arm64-v8a` and `x86`.

Please check and adjust the `abiFilters` configuration in your `build.gradle` file accordingly:

    android {
      defaultConfig {
        ndk {
          abiFilters "armeabi-v7a", "arm64-v8a", "x86"
        }
      }
    }

**Please note:** 
- `arm64-v8a`: Native libs for the `arm64-v8a` architecture are available since version **1.31.0** of Scanbot SDK. Previous versions of Scanbot SDK use `armeabi-v7a` libs since they are fully compatible with "arm64".

- `x86`: Typically the `x86` architecture can be removed for the release (production) build, since it's used only on emulators. 

- `armeabi`: The `armeabi` architecture is no longer supported.


## Tuning the Android Manifest
Since your application works with images it is highly recommended to add the property `android:largeHeap="true"` in the `<application>` element of your `AndroidManifest.xml` file.
Processing images is a memory intensive task and this property will ensure your app has enough heap allocated to avoid `OutOfMemoryError` exceptions.

    <application
      android:largeHeap="true"
      ...
    </application>


## Initialize SDK

The Scanbot SDK must be initialized before usage. Add the following code snippets to your `Application` class:

    import io.scanbot.sdk.ScanbotSDKInitializer;

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer()
          // .license(this, "YOUR_SCANBOT_SDK_LICENSE_KEY")
          .initialize(this);

        super.onCreate();
    }

Please make sure to use the `ScanbotSDKInitializer` class from the new Java package `io.scanbot.sdk.` (`net.doo.snap.ScanbotSDKInitializer` is deprecated).

## Add Scanbot SDK license

Call `ScanbotSDKInitializer#license(application, "YOUR_SCANBOT_SDK_LICENSE_KEY")` method with the provided license key:

    new ScanbotSDKInitializer()
                .license(this, "YOUR_SCANBOT_SDK_LICENSE_KEY")
                .initialize(this);

Please make sure that you have inserted the exact key as it is stated in the license file - with all line breaks.

## License checks in production apps

Please note:

If your Scanbot SDK license has expired, any call of the Scanbot SDK API will terminate your app. To prevent this you should always check for license expiration during the runtime by calling the method `scanbotSDK.isLicenseValid()`. If this method returns `false`, you should disable any usage of the Scanbot SDK functions or UI components.

We highly recommend to implement a suitable handling of this case in your app!

Example code for checking the license status

    ScanbotSDK scanbotSDK = new ScanbotSDK(activity);
    if(scanbotSDK.isLicenseValid()) {
        // Making your call into ScanbotSDK API is safe now.
    }