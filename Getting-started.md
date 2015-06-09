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

    compile 'io.scanbot:sdk-package-1:1.0.0-3'

## Initialize SDK

Scanbot SDK must be initialized before usage. Add following line to your `Application` class:

    @Override
    public void onCreate() {
        new ScanbotSDKInitializer().initialize(this);
        super.onCreate();
    }

This is the most basic set-up which will work out of the box. However, `ScanbotSDKInitializer` have more options which you might want to adjust to change appearance or behavior of your app. They're all documented, so feel free to experiment.

## Inject

Scanbot SDK uses dependency injection to facilitate quick and flexible development. As a user of our SDK, you can take advantage of that as well. For instance, to inject `DocumentProcessor` (which you'll need later), you can do following:

    public class YourActivity extends Activity {    // Any base class you like

        @Inject
        private DocumentProcessor documentProcessor;
        @Inject
        private Cleaner cleaner;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            // Feel free to put it into your base Activity (if you have any)
            RoboGuice.getInjector(this).injectMembersWithoutViews(this);

            // documentProcessor instance is initialized at this point

            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
        }

    }