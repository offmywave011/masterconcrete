All of the following steps are assuming that you are using Gradle as a build tool. It is also possible to use it with Maven, but you should adjust the steps accordingly.

## Dependencies

The Scanbot SDK is distributed through our private repository which you should specify in your `build.gradle`:

    repositories {
        jcenter()

        maven {
            url 'http://nexus.scanbot.io/nexus/content/repositories/releases/'
        }
        maven {
            url 'http://nexus.scanbot.io/nexus/content/repositories/snapshots/'
        }
    }

Afterwards, you can add a dependency to your project:

    compile "io.scanbot:sdk-package-1:$latestVersion"

See latest version in [README](https://github.com/doo/Scanbot-SDK-Examples/blob/master/README.md)

## Add license to AndroidManifest

Add the following line with the provided license key inside of your `<application>` tag in `AndroidManifest.xml`:

    <application ...>

        <meta-data android:name="SCANBOT_SDK_LICENSE_KEY" android:value="Insert your key here" />

        ...

    </application>

Please make sure that you inserted the exact key as it is stated in the license file - with all line breaks.

## Initialize SDK

The Scanbot SDK must be initialized before usage. Add the following line to your `Application` class:

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer().initialize(this);
        super.onCreate();
    }
