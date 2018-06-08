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
The Scanbot SDK uses native libraries under the hood and supports following [ABIs](https://developer.android.com/ndk/guides/arch.html): `armeabi-v7a` and `x86`.
Currently we don't provide native libs for the `arm64-v8a` architecture. But our `armeabi-v7a` libs are fully compatible with "arm64".
So to be able to run on "arm64" devices you have just to add this `abiFilters` configuration in your `build.gradle`:

    android {
      defaultConfig {
        ndk {
          abiFilters "armeabi-v7a", "x86"
        }
      }
    }

**Please note:** Typically the `x86` architecture can also be removed for the release (production) build, since it's used only on emulators. The `armeabi` architecture is no longer supported.


## Tuning the Android Manifest
Since your application works with images it is highly recommended to add the property `android:largeHeap="true"` in the `<application>` element of your `AndroidManifest.xml` file.
Processing images is a memory intensive task and this property will ensure your app has enough heap allocated to avoid `OutOfMemoryError` exceptions.

    <application
      android:largeHeap="true"
      ...
    </application>


## Initialize SDK

The Scanbot SDK must be initialized before usage. Add the following line to your `Application` class:

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer()
          // .license(this, "YOUR_SCANBOT_SDK_LICENSE_KEY")
          .initialize(this);

        super.onCreate();
    }

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