The Scanbot SDK provides ability to find and extract [Machine Readable Zone (MRZ)](https://en.wikipedia.org/wiki/Machine-readable_passport) content from ID cards, passports and travel documents.

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


#### Finder overlay

Scanbot SDK provides "Finder overlay" feature for MRZ scanner. It allows to predefine MRZ area over the `ScanbotCameraView` screen.

With this overlay MRZ scanner could skip "MRZ area search" step and can perform recognition immediately in the specified "Finder overlay" area. Scanner recognises MRZ content much faster with this approach.

To use "Finder overlay" on your screen you have to add any `View` with `android:id="@id/finder_overlay"` id to the `ScanbotCameraView` parent view. 

Parent view should not have any paddings. `ScanbotCameraView` should have `android:layout_width="match_parent"` and `android:layout_height="match_parent"` layout parameters and no paddings or margins.

"Finder overlay" view could have any margins, size, background or even child views, but it always should to be over the camera preview frame, otherwise it will throw `IllegalStateException`.

    <RelativeLayout 
        android:id="@+id/parent_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <net.doo.snap.camera.ScanbotCameraView
            android:id="@+id/camera"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

        <View
            android:id="@id/finder_overlay"
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:layout_centerInParent="true"
            android:layout_marginLeft="20dp"
            android:layout_marginRight="20dp"
            android:background="@drawable/mrz_finder_bg"/>

    </RelativeLayout>   

That is it!