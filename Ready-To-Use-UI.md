The **Ready-To-Use UI** (RTU UI) is a set of easy to integrate and customize high-level UI components (Activities) for the most common tasks in Scanbot SDK: 
- Document Scanner
- Cropping
- MRZ Scanner
- Barcode and QR-Code Scanner

The design and behavior of these ready-to-use Activities are based on our long years of experience as well as the feedback from our SDK customers.

Customization:
- UI: All colors and text resources ().
- Behavior: Enable or disable features like Multi Page, Auto Snapping, Flash Light.
- Use in workflows of your App: e.g. [Your Main Activity] => *[Document Scanner Activity]* => [Your Custom Review Activity] => *[Cropping Activity]* => [Your Custom Document Handling]...


## Dependencies

The RTU UI components are distributed as a separate package. Add it as dependency to your project:

    compile "io.scanbot:sdk-package-ui:$latestVersion"

Get the `$latestVersion` number from the [README](https://github.com/doo/Scanbot-SDK-Examples/blob/master/README.md)

## Initialize SDK with RTU UI

In order to use the RTU UI components you have to initialize the Scanbot SDK using the new `ScanbotSDKInitializer` class from the new Java package `io.scanbot.sdk`.

In your `Application` class:

```
import io.scanbot.sdk.ScanbotSDKInitializer;

@Override
public void onCreate() {
    new ScanbotSDKInitializer()
      // .license(this, "YOUR_SCANBOT_SDK_LICENSE_KEY")
      .initialize(this);

    super.onCreate();
}
```
