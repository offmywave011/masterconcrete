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