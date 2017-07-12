> I'm getting the error `Java.Lang.UnsatisfiedLinkError: No implementation found for long net.doo.snap.lib.detector.ContourDetector.ctor() (tried Java_net_doo_snap_lib_detector_ContourDetector_ctor and Java_net_doo_snap_lib_detector_ContourDetector_ctor__) occurred`

Solution: Please adjust the [Android ABI settings](Getting-started#abi-settings) of your project. 
Then clean & rebuild the project, remove the debug app from the device and reinstall it.


> I'm getting the error: `android.view.InflateException: Binary XML file line #8: Binary XML file line #8: Error inflating class net.doo.snap.camera.ScanbotCameraView`

Solution: You have to [initialize the Scanbot SDK](Getting-started#initialize-sdk) before you can use any UI components like `ScanbotCameraView` or other SDK functions.  


> I got a (trial) license key and integrated it in my app. But now the app crashes.

> I'm getting the error: `A/libc: Fatal signal 6 (SIGABRT), code -6 in tid 6627 ...`
  
Solution: Please make sure the package ID of your app matches the one of the license key we generated for you.
If you are testing our example apps with your license key, please adjust the package ID accordingly.

Please also make sure you have inserted the license key string exact as it is provided in the license file. The line breaks are important and may not be removed in the `AndroidManifest.xml`!
If you alternatively use the Java code for setting the license key on SDK initialization, please use `\n` to encode the line breaks.


> My app stopped working.
  
Solution: Either your trial license period is expired or you set the license key incorrectly.


> Your example app crashes after some time / after x images.
  
Solution: Our example apps doesn't contain a Scanbot SDK license key and run in a **trial mode (trial period of 1 minute)**. 
After the trial period is over the Scanbot SDK functions will stop working. 
The UI components (like Scanning UI) will stop working or may be terminated. You have to restart the app to get another trial period.
