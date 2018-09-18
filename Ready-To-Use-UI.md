The **Ready-To-Use UI** (RTU UI) is a set of easy to integrate and customize high-level UI components (Activities) for the most common tasks in Scanbot SDK: 
- Document Scanner - `DocumentScannerActivity`
- Cropping - `CroppingActivity`
- MRZ Scanner - `MRZScannerActivity`
- Barcode and QR-Code Scanner - `BarcodeScannerActivity`

The design and behavior of these ready-to-use Activities are based on our long years of experience as well as the feedback from our SDK customers.

## Customization

- UI: All colors and text resources (localization).
- Behavior: Enable or disable features like Multi Page, Auto Snapping, Flash Light.
- Use in workflows of your App: e.g. [Your Main Activity] => **[Document Scanner Activity]** => [Your Custom Review Activity] => **[Cropping Activity]** => [Your Custom Document Handling]...

Please note: The main idea of the RTU UI is to provide a simple-to-integrate and simple-to-customize Activity components. Due to this idea there are some limitations with the possibilities of customization.
If you need more customization options you have to implement custom Activities using our "Classical SDK UI Components".


## Integration

### Dependencies

The RTU UI components are distributed as a separate package. Add it as dependency to your project:

    compile "io.scanbot:sdk-package-ui:$latestVersion"

Get the `$latestVersion` number from the [README](https://github.com/doo/Scanbot-SDK-Examples/blob/master/README.md)

### Initialize SDK with RTU UI

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

## Examples and Java Docs

For more details about the RTU UI components please check our Example App `ready-to-use-ui`
- https://github.com/doo/Scanbot-SDK-Examples/tree/master/ScanbotSDKexample/ready-to-use-ui

and Java Docs: 
- https://doo.github.io/Scanbot-SDK-Documentation/Android/rtu-ui-api/
