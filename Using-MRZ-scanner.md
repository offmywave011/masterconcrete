The Scanbot SDK provides ability to find and extract [Machine Readable Zone (MRZ)](https://en.wikipedia.org/wiki/Machine-readable_passport) content from ID cards, passports and travel documents.

Depending on the type of document following fields can be extracted:
* document code
* first name
* last name
* issuing state or organization
* department of issuance
* nationality
* date of birth
* gender
* date of expiry
* personal number
* optional fields
* discreet issuing state or organization
* valid check digits count
* check digits count
* travel document type.


#### Getting started

Try our [Example App](https://github.com/doo/Scanbot-SDK-Examples/tree/master/ScanbotSDKexample/mrz-scanner) or check these step by step integration instructions: 

`MRZScanner` is available with the SDK Package 3. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-3:$latestVersion"

It can be used in conjunction with `ScanbotCameraView` or separately. Let's have a look at an example with `ScanbotCameraView`.

To get started, you have to undertake few steps.

**First**: Fetch MRZ specific OCR blob file.

    blobManager.fetch(blobFactory.mrzTraineddataBlob(), false);

`MRZScanner` requires a specific OCR blob file `ocrb.traineddata`. This blob file is integrated in the Scanbot SDK package 3 assets. So you don't need to configure OCR blob paths like `ocr_blobs_path` or `language_classifier_blob_path` for MRZ.

(More information about the general handling of OCR blob files can be found here: https://github.com/doo/Scanbot-SDK-Examples/wiki/OCR-document-scanning#preparing-the-data.)

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

`handleResult(MRZRecognitionResult mrzRecognitionResult)` will be triggered every time when `MRZScanner` detects MRZ area on the camera preview frame. If `mrzRecognitionResult` is not `null` and `mrzRecognitionResult.recognitionSuccessful` flag is `true` that shows MRZ content was successfully recognized. 

As a result you will get `MRZRecognitionResult` object which contains all extracted data fields like `result.documentCode`, `result.firstName`, etc.

You can now run your app and should see a simple camera preview which can scan MRZ data on your document.


#### Finder Overlay

In addition it is recommended to add a "Finder Overlay". This feature allows to predefine a MRZ area over the `ScanbotCameraView` screen. By using this overlay the MRZ scanner can skip the time-consuming step "Search for MRZ area" and perform the recognition directly in the specified "Finder Overlay" area. By using this approach the MRZ scanner recognizes and extracts the MRZ content much faster.

To use the "Finder Overlay" on your screen you can add any `View` with ID `android:id="@id/finder_overlay"` to the `ScanbotCameraView` parent view. 

The parent view should not have any paddings. `ScanbotCameraView` should have `android:layout_width="match_parent"` and `android:layout_height="match_parent"` layout parameters and no paddings or margins.

The "Finder Overlay" view could have any margins, size, background or even child views, but it should always be **over the camera preview frame**, otherwise it will throw an `IllegalStateException`.

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

That's it!