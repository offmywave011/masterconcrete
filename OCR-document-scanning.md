# OCR document scanning

For document text recognition you have to follow next steps: 

1. OCR feature is provided in the Scanbot SDK package 2. You have to add `compile 'io.scanbot:sdk-package-2:1.0.0-3'` to your Gradle dependencies.

2. SDK requires specific training data for text recognition for every language which it recognizes. You have to add url to AndroidManifest.xml for downloading OCR data.

    `<meta-data android:name="ocr_blobs_path" android:value="Insert path here" />`

    * You can download training data from the Scanbot server - replace value with `http://download.scanbot.io/di/tessdata/`. (preferred)

    * Or you can put an OCR data in the application assets directory - replace value with a path to the folder in the assets directory `your_ocr_folder_name_in_assets/`.

3. Add `DocumentProcessor`, `BlobFactory` and `BlobManager` injects:

    `@Inject`

    `private DocumentProcessor documentProcessor;`

    `@Inject`

    `private BlobManager blobManager;`

    `@Inject`

    `private BlobFactory blobFactory;` 
 
    Get OCR data blobs for the language you want to perform OCR, e.g. english:
    
    `blobFactory.ocrLanguageBlobs(Language.ENG)`

    This method will return a collection of `Blob` instances, which have to be downloaded. Call `blobManager.fetch(blob, false);` for all of them. You have to call `fetch(blob, false)` even for `Blob`s in assets. All training data files will be cached in internal application folder. 

4. When all required blobs will be downloaded, you can start `DocumentDraft` processing in `DocumentProcessor`. It will execute text recognition and compose PDF file with the recognized text in it.
    * For all `DocumentDraft`s set OCR status `OcrStatus.PENDING`.
    
    `draft.getDocument().setOcrStatus(OcrStatus.PENDING);`
   
    * Call `documentProcessor.processDocument(draft)` in `DocumentProcessor`.
    * As a result `DocumentProcessor` returns a `DocumentProcessingResult` that contains generated `Document` object. You can call `documentProcessingResult.getDocument().getOcrText()` and will get a recognized text.

`DocumentProcessor` supports several OCR statuses that you can set to `DocumentDraft`:
* `OcrStatus.PENDING` - OCR well be performed only if a preference PreferencesConstants.PERFORM_OCR is true and PreferencesConstants.OCR_ONLY_WHILE_CHARGING is false (or true and the device is charging).
* `OcrStatus.PENDING_FORCED` - OCR will be performed. Ignores all preferences flags.
* `OcrStatus.PENDING_ON_CHARGER` - OCR will be performed only if device is charging. Ignores all preferences flags. 