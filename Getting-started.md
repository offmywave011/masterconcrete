All of the following steps are assuming that you are using Gradle as a build tool. It is also possible to use it with Maven, but you should adjust the steps accordingly.

## Dependencies

The Scanbot SDK is distributed through our private repository which you should specify in your `build.gradle`:

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
Currently we don't provide native libs the `arm64-v8a` architecture. But our `armeabi-v7a` libs are fully compatible with "arm64".
So to be able to run on "arm64" devices you have just to add this `abiFilters` configuration in your `build.gradle`:

    ...
    android {
      ...
      defaultConfig {
        ...
        ndk {
          abiFilters "armeabi-v7a", "x86"
        }
      }
    }


## Add license to AndroidManifest

Add the following line with the provided license key inside of your `<application>` tag in `AndroidManifest.xml`:

    <application ...>

        <meta-data android:name="SCANBOT_SDK_LICENSE_KEY" android:value="Insert your key here" />

        ...

    </application>

Please make sure that you have inserted the exact key as it is stated in the license file - with all line breaks.

## Initialize SDK

The Scanbot SDK must be initialized before usage. Add the following line to your `Application` class:

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer().initialize(this);
        super.onCreate();
    }

## License checks in production apps

Please note:

If your Scanbot SDK license has expired, any call of the Scanbot SDK API will terminate your app. To prevent this you should always check for license expiration during the runtime by calling the method `scanbotSDK.isLicenseActive()`. If this method returns `false`, you should disable any usage of the Scanbot SDK functions or UI components.

We highly recommend to implement a suitable handling of this case in your app!

Example code for checking the license status

    ScanbotSDK scanbotSDK = new ScanbotSDK(activity);
    if(scanbotSDK.isLicenseActive()) {
        // Making your call into ScanbotSDK API is safe now.
    }