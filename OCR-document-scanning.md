## Setting up project

OCR feature is provided in the Scanbot SDK package 2. You have to add following dependency:

    compile "io.scanbot:sdk-package-2:$latestVersion"

Then, add those permissions to your AndroidManifest.xml

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

OCR module requires training data for text recognition for every language which it uses (you can decide on which languages to use). You have to add url to AndroidManifest.xml for downloading OCR data.

    <meta-data android:name="ocr_blobs_path" android:value="Insert path here" />

As for now, you have 2 options:

* Download training data from the Scanbot server - replace value with `http://download.scanbot.io/di/tessdata/`. (preferred)

* Put an OCR data in the application assets directory - replace value with a path to the folder in the assets directory `folder_with_traineddata/`. You can get trained data files from Tesseract website: https://code.google.com/p/tesseract-ocr/downloads/list

## Preparing the data

Use `BlobFactory` and `BlobManager` dependencies:

    BlobManager blobManager = scanbotSDK.blobManager();
    BlobFactory blobFactory = scanbotSDK.blobFactory();
 
Get OCR blobs for the language you want to perform OCR on, e.g. english:
    
    blobFactory.ocrLanguageBlobs(Language.ENG)

This method will return a collection of `Blob` instances which required for OCR. All of them need to be fetched either from server or from assets. Call `blobManager.fetch(blob, false);` to do that. All training data files will be cached in internal application folder.

## Preparing the pages

OCR engine works with prepared optimized images. More specifically it takes an image from a page which is stored as `Page.ImageType.OPTIMIZED`. By default `PageFactory` does not provide this image for you, so it's up to you to pre-process the image and save it.

Typically it's done like this:

    File imageFile = pageStoreStrategy.getImageFile(page.getId(), Page.ImageType.ORIGINAL);

    Bitmap result = contourDetector.processImageF(
                    imageFile.getPath(),
                    polygon,
                    ContourDetector.IMAGE_FILTER_NONE
    );

    File optimizedFile = pageStoreStrategy.getImageFile(page.getId(), Page.ImageType.OPTIMIZED);

    result.compress(Bitmap.CompressFormat.JPEG, 80, new FileOutputStream(optimizedFile));

## Recognizing text

When all required blobs will be downloaded, you can create documents using `TextRecognition`. It will do text recognition and compose PDF file as an option. `TextRecognition` must be created for each text recognition session.

To perform recognition with composed PDF use:

    scanbotSDK.textRecognition().withPDF(Language defaultLanguage, Document document, List<Page> pages);

Where:
* `defaultLanguage` to perform recognition with. Use it to skip part with language detection and only if you are sure with what language recognition should be preformed. Can be null.
* `document` into which composed PDF will be written.
* `pages` to perform OCR on.

It returns `OcrResult` which consists from recognized text and composed PDF.

To get just recognized text use:

    scanbotSDK.textRecognition().withoutPDF(Language defaultLanguage, List<Page> pages);