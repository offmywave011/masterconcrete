The Scanbot SDK provides the ability to scan and extract content from SEPA pay forms.

Following fields are supported:
- IBAN
- BIC 
- Transfer Amount 
- Reference Number
- Sender IBAN / Name

Try our [PayForm Scanner Example App](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/payform-scanner) or check the following step by step integration instructions.

### Step 1 - Add PayForm Scanner Feature as Dependency

`PayFormScanner` is available with the SDK Package 3. You have to add the following dependency for it:

    api "io.scanbot:sdk-package-3:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at example with `ScanbotCameraView`.


### Step 2 - Prepare the OCR language blobs and PayForm specific blob files
The PayForm Scanner is based on the OCR Feature of Scanbot SDK. Please check the [[Optical Character Recognition]] docs for more details.

In order to use the PayForm Scanner you need to prepare the German and English OCR language files as well as some internal PayForm Recognizer specific blob files. Place the `deu.traineddata` and `eng.traineddata` files in the assets sub-folder `assets/ocr_blobs/` of your app.

Then on initialization of the SDK call the `prepareOCRLanguagesBlobs(true)`and the `preparePayFormBlobs(true)` methods:
```
import io.scanbot.sdk.ScanbotSDKInitializer;

new ScanbotSDKInitializer()
      .prepareOCRLanguagesBlobs(true)
      .preparePayFormBlobs(true)
      ...
      .initialize(this);
```

### Step 3 - Get `PayFormScanner` instance from `ScanbotSDK` and attach it to `ScanbotCameraView`

    ScanbotSDK scanbotSDK = new ScanbotSDK(this);
    final PayFormScanner payFormScanner = scanbotSDK.payFormScanner();
    PayFormScannerFrameHandler payFormScannerFrameHandler = PayFormScannerFrameHandler.attach(cameraView, payFormScanner);

### Step 4 - Add result handler for `PayFormScannerFrameHandler`:

    payFormScannerFrameHandler.addResultHandler(new PayFormScannerFrameHandler.ResultHandler() {
        @Override
        public boolean handleResult(DetectionResult detectionResult) {
            if (detectionResult != null && detectionResult.form.isValid()) {
                List<RecognizedField> fields = payFormScanner.recognizeForm(detectionResult.lastFrame, detectionResult.frameWidth, detectionResult.frameHeight, 0);
            }
            return false;
        }
    });

`handleResult(DetectionResult detectionResult)` will be triggered every time when `PayFormScanner` detects pay form on the camera preview frame. After pay form detection you have to call `payFormScanner.recognizeForm(byte[] previewFrame, int width, int height, int orientation)`. As a result you will get `List<RecognizedField> fields`. It is a list of recognized pay form fields (IBAN, BIC, etc.). 

That is it! You can start your app and you should see the camera preview which can scan SEPA pay forms.
