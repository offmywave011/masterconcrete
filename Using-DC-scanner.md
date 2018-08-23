Scanbot SDK provides ability to find and extract content form Disability Certificates.

#### Getting started

`DCScanner` is available with the SDK Package 4. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-4:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at example with `ScanbotCameraView`.

To get started, you have to undertake few steps.

**First**: Fetch english language OCR blob.

    try {
        Collection<Blob> blobs = blobFactory.ocrLanguageBlobs(Language.ENG);

        for (Blob blob : blobs) {
            blobManager.fetch(blob, false);
        }
    } catch (IOException e) {
        logger.logException(e);
    }

More information about blobs fetching you can find here: https://github.com/doo/Scanbot-SDK-Examples/wiki/OCR-document-scanning#preparing-the-data.

**Second**: Get `DCScanner` instance from `ScanbotSDK` and attach it to `ScanbotCameraView`

    ScanbotSDK scanbotSDK = new ScanbotSDK(this);
    final DCScanner dcScanner = scanbotSDK.dcScanner();
    DCScannerFrameHandler dcScannerFrameHandler = DCScannerFrameHandler.attach(cameraView, dcScanner);

**Third**: Add result handler for `DCScannerFrameHandler`:

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

