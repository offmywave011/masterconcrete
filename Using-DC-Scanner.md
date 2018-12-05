The Scanbot SDK provides the ability to find and extract content from German **Disability Certificates** (DC) forms.

Try our [DC Scanner Example Apps](https://github.com/doo/scanbot-sdk-example-android/tree/master/ScanbotSDKexample/dc-scanner) or check the following step by step integration instructions.

### Step 1 - Add DC Feature as Dependency
`DCScanner` is available with the SDK Package 4. You have to add the following dependency for it:

    api "io.scanbot:sdk-package-4:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at example with `ScanbotCameraView`.

### Step 2 - Prepare the OCR language blob file

The DC Recognizer is based on the OCR Feature of Scanbot SDK. Please check the [[Optical-Character-Recognition]] docs for more details.

In order to use the DC Recognizer you need to prepare the English OCR language file.
Place the `eng.traineddata` file in the assets sub-folder `assets/ocr_blobs/` of your app. 

Then on initialization of the SDK call the `prepareOCRLanguagesBlobs(true)` method:

import io.scanbot.sdk.ScanbotSDKInitializer;
```
new ScanbotSDKInitializer()
      .prepareOCRLanguagesBlobs(true)
      ...
      .initialize(this);
```

### Step 3 - Get `DCScanner` instance from `ScanbotSDK` and attach it to `ScanbotCameraView`

    ScanbotSDK scanbotSDK = new ScanbotSDK(this);
    final DCScanner dcScanner = scanbotSDK.dcScanner();
    DCScannerFrameHandler dcScannerFrameHandler = DCScannerFrameHandler.attach(cameraView, dcScanner);

### Step 4 - Add result handler for `DCScannerFrameHandler`

    dcScannerFrameHandler.addResultHandler(new DCScannerFrameHandler.ResultHandler() {
            @Override
            public boolean handleResult(DisabilityCertificateRecognizerResultInfo resultInfo) {
                if (resultInfo != null && resultInfo.recognitionSuccessful) {
                    // DC content is recognised. Do whatever you want with result.
                }
                return false;
            }
    });

`handleResult(DisabilityCertificateRecognizerResultInfo resultInfo)` will be triggered every time when `DCScanner` detects document content on the camera preview frame. If `resultInfo` is not `null` and `resultInfo.recognitionSuccessful` flag is `true` that shows DC content was successfully recognised. 

As a result you will get `DisabilityCertificateRecognizerResultInfo` object that contains all extracted data:
* `DisabilityCertificateInfoBox` checkboxes with content types, states and confidence values
* List of `DateRecord` objects with recognised dates, date types, recognitionConfidenceValues and validationConfidenceValues

That is it! You can start your app and you should see the camera preview which can scan Disability Certificate document.

