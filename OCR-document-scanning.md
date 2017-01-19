## Setting up project

The OCR feature is provided in our Scanbot SDK package 2. You have to add the following dependency for it:

    compile "io.scanbot:sdk-package-2:$latestVersion"

Then, you have to add these permissions to your AndroidManifest.xml

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

The OCR module requires training data for text recognition for every language which it uses (you can decide on which languages to use). You have to add the specific url to AndroidManifest.xml for downloading OCR data.

    <meta-data android:name="ocr_blobs_path" android:value="Insert path here" />

As for now, you have 2 options:

* Download training data from the Scanbot server - replace the value with `http://download.scanbot.io/di/tessdata/`. (preferred)

* Put the OCR data in the application assets directory - replace the value with a path to the folder in the assets directory `folder_with_traineddata/`. You can get trained data files from the Tesseract website: https://code.google.com/p/tesseract-ocr/downloads/list

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