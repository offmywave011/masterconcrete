All following steps are assuming that you're using Gradle as a build tool. It is also possible to use it with Maven, but you should adjust steps accordingly.

## Dependencies

Scanbot SDK is distributed through our private repository which you should specify in your `build.gradle`:

    repositories {
        jcenter()

        maven {
            url 'http://nexus.doo.net/nexus/content/repositories/releases/'
        }
        maven {
            url 'http://nexus.doo.net/nexus/content/repositories/snapshots/'
        }
    }

Afterwards, you can add dependency to your project:

    compile "io.scanbot:sdk-package-1:$latestVersion"

See latest version in [README](https://github.com/doo/Scanbot-SDK-Examples/blob/master/README.md)

## Add license to AndroidManifest

Add the following line with license key inside of your `<application>` tag in `AndroidManifest.xml`:

    <application ...>

        <meta-data android:name="SCANBOT_SDK_LICENSE_KEY" android:value="Insert your key here" />

        ...

    </application>

Please, make sure that you inserted the key as it is in the license file - with all line breaks.

## Initialize SDK

Scanbot SDK must be initialized before usage. Add following line to your `Application` class:

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer().initialize(this);
        super.onCreate();
    }

This is the most basic set-up which will work out of the box. However, `ScanbotSDKInitializer` have more options which you might want to adjust to change appearance or behavior of your app. They're all documented, so feel free to experiment.