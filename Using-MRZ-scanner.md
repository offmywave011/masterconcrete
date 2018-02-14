Scanbot SDK provides ability to find and extract Machine Readable Zone content form travel documents.

#### Getting started

`MRZScanner` is available with the SDK Package 3. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-3:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at example with `ScanbotCameraView`.

To get started, you have to undertake few steps.

**First**: Fetch MRZ specific OCR blob.

    blobManager.fetch(blobFactory.mrzTraineddataBlob(), false);

You don't need to provide `ocr_blobs_path` and `language_classifier_blob_path` urls for fetching MRZ blob. This blob is integrated in the Scanbot SDK package 3 assets.

More information about blobs fetching you can find here: https://github.com/doo/Scanbot-SDK-Examples/wiki/OCR-document-scanning#preparing-the-data.

**Second**: Get `MRZScanner` instance from `ScanbotSDK` and attach it to `ScanbotCameraView`

    ScanbotSDK scanbotSDK = new ScanbotSDK(this);
    final MRZScanner mrzScanner = scanbotSDK.mrzScanner();
    MRZScannerFrameHandler mrzScannerFrameHandler = MRZScannerFrameHandler.attach(cameraView, mrzScanner);

**Third**: Add result handler for `MRZScannerFrameHandler`:

    mrzScannerFrameHandler.addResultHandler(new MRZScannerFrameHandler.ResultHandler() {
        @Override
        public boolean handleResult(MRZRecognitionResult mrzRecognitionResult) {
            if (mrzRecognitionResult != null && mrzRecognitionResult.recognitionSuccessful) {
                // MRZ is recognised. Do whatever you want with result.
            }
            return false;
        }
    });

`handleResult(MRZRecognitionResult mrzRecognitionResult)` will be triggered every time when `MRZScanner` detects MRZ area on the camera preview frame. If `mrzRecognitionResult` is not `null` and `mrzRecognitionResult.recognitionSuccessful` flag is `true` that shows MRZ content was successfully recognised. 

As a result you will get `MRZRecognitionResult` object that contains all extracted data:
* document code
* first name
* last name
* issuing state or organisation
* department of issuance
* nationality
* date of birth
* gender
* date of expiry
* personal number
* optional fields
* discreet issuing state or organisation
* valid check digits count
* check digits count
* travel document type.

That is it! You can start your app and you should see the camera preview which can scan MRZ data on your document.
