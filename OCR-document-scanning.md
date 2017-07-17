## Setting up project

The OCR feature is provided in our Scanbot SDK package 2. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-2:$latestVersion"

Then, you have to add these permissions to your AndroidManifest.xml

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

The OCR module of the Scanbot SDK requires training data files (aka. tessdata) for every language to perform text recognition. For each desired OCR language a corresponding training data file must be installed in the internal working directory of the Scanbot SDK. Furthermore the special data file `osd.traineddata` is required and must be installed. It is used for orientation and script detection. The Scanbot SDK ships with no training data files by default to keep the package small in size.

You have to specify following source URIs in the `AndroidManifest.xml` where Scanbot SDK can fetch the training data files:

    <meta-data android:name="ocr_blobs_path" android:value="https://github.com/tesseract-ocr/tessdata/raw/3.04.00" />
    <meta-data android:name="language_classifier_blob_path" android:value="https://download.scanbot.io/di/android" />

The Scanbot SDK will download OCR files asynchronously via Android `DownloadManager`.

Alternatively, you can define a local assets folder of your app as source URI for `ocr_blobs_path`:

    <meta-data android:name="ocr_blobs_path" android:value="my_traineddata/" />

In this case you have to download required OCR traineddata language files manually and place them in the application assets directory of your project: `assets/my_traineddata/`. The Scanbot SDK will fetch the OCR language files from this assets folder.

**Please note:** The current Scanbot SDK supports training data files of Tesseract version **3.0x** only. 
Please find a list of all supported languages in the [Tesseract wiki](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files#data-files-for-version-304305).


## Preparing the data

Use the `BlobFactory` and `BlobManager` dependencies:

    BlobManager blobManager = scanbotSDK.blobManager();
    BlobFactory blobFactory = scanbotSDK.blobFactory();
 
Get the OCR blobs for the language you want to perform OCR on, e.g. english:
    
    blobFactory.ocrLanguageBlobs(Language.ENG)

This method will return a collection of `Blob` instances which are required for OCR. All of them need to be fetched either from a server or from assets. Call `blobManager.fetch(blob, false);` to do that. All training data files will be cached in an internal application folder.

## Preparing the pages

The OCR engine works with prepared optimized images. More specifically, it takes an image from a page which is stored as `Page.ImageType.OPTIMIZED`. By default, `PageFactory` does not provide this image for you, so it is up to you to pre-process the image and save it.

Typically it is done like this:

    File imageFile = pageStoreStrategy.getImageFile(page.getId(), Page.ImageType.ORIGINAL);

    Bitmap result = contourDetector.processImageF(
                    imageFile.getPath(),
                    polygon,
                    ContourDetector.IMAGE_FILTER_NONE
    );

    File optimizedFile = pageStoreStrategy.getImageFile(page.getId(), Page.ImageType.OPTIMIZED);

    result.compress(Bitmap.CompressFormat.JPEG, 80, new FileOutputStream(optimizedFile));

## Recognizing text

When all required blobs are downloaded, you can create documents using `TextRecognition`. It will run the text recognition and compose a PDF file as an option. `TextRecognition` must be created for each text recognition session.

To perform recognition with a composed PDF use:

    scanbotSDK.textRecognition().withPDF(Language defaultLanguage, Document document, List<Page> pages);

Where:
* `defaultLanguage` to perform recognition with. Use it to skip the part with language detection and only if you are sure with which language the recognition should be performed. It also can be null.
* `document` into which a composed PDF will be written.
* `pages` to perform OCR on.

It returns the `OcrResult` which consists of recognized text and a composed PDF.

To get the recognized text only, use:

    scanbotSDK.textRecognition().withoutPDF(Language defaultLanguage, List<Page> pages);