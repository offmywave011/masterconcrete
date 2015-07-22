## Setting up project

OCR feature is provided in the Scanbot SDK package 2. You have to add following dependency:

    compile "io.scanbot:sdk-package-2:$latestVersion"

OCR module requires training data for text recognition for every language which it uses (you can decide on which languages to use). You have to add url to AndroidManifest.xml for downloading OCR data.

    <meta-data android:name="ocr_blobs_path" android:value="Insert path here" />

As for now, you have 2 options:

* Download training data from the Scanbot server - replace value with `http://download.scanbot.io/di/tessdata/`. (preferred)

* Put an OCR data in the application assets directory - replace value with a path to the folder in the assets directory `folder_with_traineddata/`. You can get trained data files from Tesseract website: https://code.google.com/p/tesseract-ocr/downloads/list

## Preparing the data

Inject `DocumentProcessor`, `BlobFactory` and `BlobManager` dependencies:

    @Inject
    private DocumentProcessor documentProcessor;
    @Inject
    private BlobManager blobManager;
    @Inject
    private BlobFactory blobFactory;
 
Get OCR blobs for the language you want to perform OCR on, e.g. english:
    
    blobFactory.ocrLanguageBlobs(Language.ENG)

This method will return a collection of `Blob` instances which required for OCR. All of them need to be fetched either from server or from assets. Call `blobManager.fetch(blob, false);` to do that. All training data files will be cached in internal application folder.

## Recognizing text

When all required blobs will be downloaded, you can create documents using `DocumentProcessor`. It will do text recognition and compose PDF file with the recognized text in it.

There is one difference though. You need to explicitly specify in `DocumentDraft` that OCR is desired. To do so, you have to set proper OCR status:
    
    draft.getDocument().setOcrStatus(OcrStatus.PENDING);
   
As a result `DocumentProcessor` returns a `DocumentProcessingResult` that contains generated `Document` object. You can call `documentProcessingResult.getDocument().getOcrText()` and will get a recognized text.

`DocumentProcessor` supports several OCR statuses that you can set to `DocumentDraft`:
* `OcrStatus.PENDING` - OCR well be performed only if a preference PreferencesConstants.PERFORM_OCR is true and PreferencesConstants.OCR_ONLY_WHILE_CHARGING is false (or true and the device is charging).
* `OcrStatus.PENDING_FORCED` - OCR will be performed. Ignores all preferences flags.
* `OcrStatus.PENDING_ON_CHARGER` - OCR will be performed only if device is charging. Ignores all preferences flags. 