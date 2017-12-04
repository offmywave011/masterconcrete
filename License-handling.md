## License checks in production apps
**Please note:**
If your Scanbot SDK license has expired, any call of the Scanbot SDK API will terminate your app or result in a  RuntimeException (no-op error). To prevent this you should always check for license expiration during the runtime by calling the method `new ScanbotSDK(activity).isLicenseValid()`. If this method returns `false`, you should disable any usage of the Scanbot SDK functions or UI components.

We highly recommend to implement a suitable handling of this case in your app!

## Updating the license in production apps
**Please also note:**
To renew an expired license or extend a valid license with new Scanbot SDK features, you will have to update your app in the Play Store.
The expiration date and the feature list of a license are an encrypted data part of the license key string. Which means a renewal or extension of a license will cause a new license key string to be generated.
