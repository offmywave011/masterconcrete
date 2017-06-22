Scanbot SDK provides ability to scan and extract content (like IBAN, BIC, receiver name, sender name, transfer amount, reference number) form SEPA pay forms.

#### Getting started

`PayFormScanner` is available with the SDK Package 3. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-3:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at example with `ScanbotCameraView`.

To get started, you have to undertake few steps.

**First**: Fetch english and german language OCR blobs, language detector blobs and banks data blob.

    Collection<Blob> blobs = blobFactory.ocrLanguageBlobs(Language.DEU);
    blobs.addAll(blobFactory.ocrLanguageBlobs(Language.ENG));
    blobs.addAll(blobFactory.languageDetectorBlobs());
    blobs.add(blobFactory.bankDataBlob());

    for (Blob blob : blobs) {
        blobManager.fetch(blob, false);
    }

More information about blobs fetching you can find here: https://github.com/doo/Scanbot-SDK-Examples/wiki/OCR-document-scanning#preparing-the-data

**Second**: Get `PayFormScanner` instance from `ScanbotSDK` and attach it to `ScanbotCameraView`

    ScanbotSDK scanbotSDK = new ScanbotSDK(this);
    final PayFormScanner payFormScanner = scanbotSDK.payFormScanner();
    PayFormScannerFrameHandler payFormScannerFrameHandler = PayFormScannerFrameHandler.attach(cameraView, payFormScanner);

**Third**: Add result handler for `PayFormScannerFrameHandler`:

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
