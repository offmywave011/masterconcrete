Since version **1.40.0** the Scanbot SDK for Android provides a simple and convenient API ([`OpticalCharacterRecognizer`](https://scanbotsdk.github.io/documentation/android/api/io.scanbot.sdk/io/scanbot/sdk/ocr/OpticalCharacterRecognizer.html)) to run Optical Character Recognition (OCR) on images.
As result you can get
- a searchable PDF document with the recognized text layer (aka. sandwiched PDF document)
- recognized text as plain text
- bounding boxes of all recognized paragraphs, lines and words
- text results and confidence values for each bounding box.

The Scanbot OCR feature is based on the Tesseract OCR engine with some modifications and enhancements.
The OCR engine supports a wide variety of languages. For each desired language a corresponding OCR training data file (`.traineddata`) must be provided.
Furthermore the special data file `osd.traineddata` is required (used for orientation and script detection).
The Scanbot SDK package contains no language data files to keep the SDK small in size. You have to download and include the desired files in your app.


## Step 1 - Add OCR Feature as Dependency
The OCR feature is provided in **Scanbot SDK Package II**. You have to add the corresponding dependency for Package II `io.scanbot:sdk-package-2` or higher in your `build.gradle`:
```
api "io.scanbot:sdk-package-2:$latestVersion"
```


## Step 2 - Download and Provide OCR Language Files
Please find a list of all supported OCR languages and download links on this [Tesseract wiki page](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files#data-files-for-version-304305).
The current Scanbot SDK supports `.traineddata` files of Tesseract version **3.04/3.05** only!

Download the files and place them in the assets sub-folder `assets/ocr_blobs/` of your app.
Example:
- `assets/ocr_blobs/eng.traineddata` // english language file
- `assets/ocr_blobs/deu.traineddata` // german language file
- `assets/ocr_blobs/osd.traineddata` // required special data file


## Step 3 - Initialization
In order to initialize the Scanbot SDK with provided OCR data files, you have to call the `prepareOCRLanguagesBlobs(true)` on initialization of the SDK.
In your `Application` class:
```
import io.scanbot.sdk.ScanbotSDKInitializer;

new ScanbotSDKInitializer()
      .prepareOCRLanguagesBlobs(true)
      ...
      .initialize(this);
```

Then get an instance of the `OpticalCharacterRecognizer` from `ScanbotSDK`.
In your `Activity` or `Service` class:
```
import io.scanbot.sdk.ScanbotSDK;
import io.scanbot.sdk.ocr.OpticalCharacterRecognizer;

OpticalCharacterRecognizer ocrRecognizer = new ScanbotSDK(this).ocrRecognizer();
```


## Step 4 - Run OCR

### On arbitrary images
You can OCR on arbitrary image files (JPG or PNG) provided as file URIs:
```
import io.scanbot.sdk.process.PDFPageSize;
import net.doo.snap.entity.Language;

List<android.net.Uri> imageFileUris = ...;  // ["file:///some/path/file1.jpg", "file:///some/path/file2.jpg", ...]
Set<Language> languages = new HashSet<>();
languages.add(Language.ENG);

// with PDF as result:
OcrResult result = ocrRecognizer.recognizeTextWithPdfFromUris(imageFileUris, PDFPageSize.FIXED_A4, languages);

// without PDF:
OcrResult result = ocrRecognizer.recognizeTextFromUris(imageFileUris, languages);
```

### On RTU UI `Page` objects
If you are using our **RTU UI Components**, you can use the corresponding methods to pass a list of RTU UI `Page` objects:
```
import io.scanbot.sdk.persistence.Page;
import io.scanbot.sdk.process.PDFPageSize;
import net.doo.snap.entity.Language;

List<Page> pages = ...; // e.g. snap some pages via RTU UI DocumentScannerActivity
Set<Language> languages = new HashSet<>();
languages.add(Language.DEU);

// with PDF as result:
OcrResult result = ocrRecognizer.recognizeTextWithPdfFromPages(pages, PDFPageSize.FIXED_A4, languages);

// without PDF:
OcrResult result = ocrRecognizer.recognizeTextFromPages(pages, languages);
```
Please note: The OpticalCharacterRecognizer uses the **document image** (cropped image) of a `Page` object. So make sure all `Page` objects contain document images.


## OCR Results
In case of running OCR with PDF the result object contains the searchable PDF document with the recognized text layer (aka. sandwiched PDF document):
```
File pdfFile = result.sandwichedPdfDocumentFile;
```

In all cases the OCR result also contains the recognized plain text as well as the bounding boxes and text results of recognized paragraphs, lines and words:
```
String text = result.recognizedText; // recognized plain text
// bounding boxes and text results of recognized paragraphs, lines and words:
List<OcrResultBlock> paragraphs = result.paragraphs;
List<OcrResultBlock> lines = result.lines;
List<OcrResultBlock> words = result.words;
```

See the API reference of the [`OcrResult`](https://scanbotsdk.github.io/documentation/android/api/net.doo.snap/net/doo/snap/process/OcrResult.html) class for more details.
