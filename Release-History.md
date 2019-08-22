## What is the latest version of the Scanbot SDK for Android?

The current version of the SDK is **1.54.0**


## Release History and Changelog

### Version 1.54.0 (19 Aug 2019)
- 🐞 Bug fixes:
  * Fixed OCR issues with some document images.
  * Fixed crashes with some image filters, like `BACKGROUND_CLEAN`, `DEEP_BINARIZATION`, `LOW_LIGHT_BINARIZATION`, etc.
  * Fixed finder view of the RTU UI EHIC Scanner (`HealthInsuranceCardScannerActivity`).
- 🚙 Under the hood:
  * Upgraded to OpenCV 3.4.7


### Version 1.53.0 (15 Aug 2019)
- 🎉 New:
  * New recognizer for the European Health Insurance Cards (EHIC). 
    Provides live detection and data extraction of all fields on the back of the card. 
    Available as **Classical SDK Components** (`HealthInsuranceCardScanner`, `HealthInsuranceCardScannerFrameHandler`) as well as the **Ready-To-Use UI Component** (`HealthInsuranceCardScannerActivity`). 
    See our updated example apps on GitHub for easy integration.
  * New QR- & Barcode Scanner as **beta** feature which provides:
    - better detection and extraction of 1D and 2D barcodes, especially Data Matrix and PDF 417 codes
    - support for multiple barcode detection
    - out-of-the-box parsers, like German Medical Plans (Medikationsplan) based on Data Matrix, ID Cards or US Driver Licenses (AAMVA format) both based on PDF 417.
  * See the new example app [`beta-barcode-scanner`](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/beta-barcode-scanner).
  * Please note that this new QR- & Barcode Scanner component is a **BETA** feature and is still under active development and improvement. We will try to keep the API as stable as possible. However, please note that we can't guarantee that.
- ⚠️ Breaking Changes:
  * The API of the `BarcodeScanningResult` class has changed a little. It now contains multiple results as a list of `BarcodeItem`s.
- 🐞 Bug fixes:
  * Fixed camera issues on OnePlus 7 Pro devices.


### Version 1.52.0 (18 Jun 2019)
- 🚀 Improvements:
  * Removed dependencies to the deprecated library `net.doo:doo-datamining-tools-android`.
- 🐞 Bug fixes:
  * Removed permission `READ_PHONE_STATE` used in a sub library of the Scanbot SDK.


### Version 1.51.0 (6 Jun 2019)
- 🎉 New:
  * Added a new config property in the RTU UI Document Scanner Component: `DocumentScannerConfiguration.setMaxNumberOfPages(number)` - Maximum number of pages to scan.


### Version 1.50.0 (6 May 2019)
- 🎉 New:
  * Workflows - New RTU UI Scanning Components. See the [[Workflows]] section for more details.
  * New black & white image filter `LOW_LIGHT_BINARIZATION` - Binarization filter primarily intended to use on low-contrast documents with hard shadows.
  * Added new config properties for the RTU UI Document Scanner Component:
      + `DocumentScannerConfiguration.setDocumentImageSizeLimit(height, width)` - to limit the size of the document image.
      + `DocumentScannerConfiguration.setShutterButtonHidden(boolean)` - to hide the shutter button.
  * Added native libs (.so) for the `x86_64` architecture.
  * Added text orientation recognizer. See the class `TextOrientationRecognizer`.
- 🚀 Improvements:
  * OCR - Upgraded the OCR engine to Tesseract v4.00. Improved recognition speed and quality. Please also note the Breaking Changes below.
  * Improved performance of the images filter `DEEP_BINARIZATION`.
  * Added a new method `detectAndRecognizeDCBitmap(..)` in `DisabilityCertificateRecognizer` as an alternative detection approach to the `recognizeDCBitmap(..)` method.
- ⚠️ Breaking Changes:
  * OCR Language Files - If you use the OCR feature of the Scanbot SDK or some recognizer components which are based on OCR, please upgrade the OCR language files to Tesseract 4.00.
    See the [[Optical Character Recognition]] section for more details.
  * OCR API - OCR results per page. See `OcrResult.ocrPages`.
- 🚙 Under the hood:
  * Upgraded some libs to latest versions: OpenCV 3.4.5, LibPNG 1.6.36, LibTIFF 4.0.10, latest BoringSSL version.
- 🐞 Bug fixes:
  * Fixed a bug with the Magnifier view in RTU Cropping UI (the Magnifier was stuck in a corner)
  * Various minor bug fixes and improvements.


### Version 1.41.0 (31 Jan 2019)
- 🎉 New:
  * Implemented new feature "Required Aspect Ratios" in **Classical Components** to restrict document detection to given aspect ratios
      - Check out the example app [camera-view-aspect-ratio-finder](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/camera-view-aspect-ratio-finder)
  * Implemented new image filter types `IMAGE_FILTER_OTSU_BINARIZATION`, `IMAGE_FILTER_DEEP_BINARIZATION` and `IMAGE_FILTER_EDGE_HIGHLIGHT`

- 🚀 Improvements:
  * Improved recognition of pay cheques (`ChequeScanner`)

- 🐞 Bug fixes:
  * Various minor bug fixes and improvements


### Version 1.40.0 (5 Dec 2018)
- 🎉 New:
  * Support for [AndroidX](https://developer.android.com/jetpack/androidx/) libraries.
  * PDF Renderer (`io.scanbot.sdk.process.PDFRenderer`) - A new simple and convenient API for PDF rendering. Now with support for PDF page sizes (e.g. A4, US Letter, etc.) See the docs [[Creating PDF Documents]] for more details.
  * OCR Recognizer/Renderer (`io.scanbot.sdk.ocr.OpticalCharacterRecognizer`) - A new simple and convenient API for OCR and PDF+OCR rendering. See the docs [[Optical Character Recognition]] for more details.

- ⚠️ Breaking changes:
  * **AndroidX**: Since AndroidX fully replaces the Android Support Libraries (`com.android.support.*`, etc), you will have to migrate your project to AndroidX. See ["Migrating to AndroidX"](https://developer.android.com/jetpack/androidx/migrate) for more details.
  * Internal Storage: The Scanbot SDK now uses the internal storage by default, which is more secure and do not require storage permission prompts. See the docs [[Storage]] for more details about the storage system. Please note: In case your app is using the storage for RTU UI Pages as permanent storage, make sure to implement a suitable migration (E.g. move the archived `Page` images from external storage folder to the new internal storage. Or alternatively overwrite the storage to the external folder again).
  * OCR blobs handling: A new mechanism and API to handle OCR language files as well as other blob files (MRZ blobs, etc) as assets. It replaces the obsolete mechanism based on `fetch()` methods (`blobManager.fetch(..)`). See the docs [[Optical Character Recognition]] for more details.

- 🚀 Improvements:
  * Support for `RGBA_F16` Bitmaps (https://developer.android.com/reference/android/graphics/Bitmap.Config#RGBA_F16). Auto conversion to compatible Bitmaps.
  * Updated some third-party libs to the latest versions (e.g. Kotlin 1.3.0, etc).
  * Cleaned up some dependencies to avoid potential conflicts on implementation.

### Version 1.39.52.2 (22 Aug 2019)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.52.0, but **without AndroidX** dependencies.
- Fixed crashes with some image filters, like `BACKGROUND_CLEAN`, `DEEP_BINARIZATION`, `LOW_LIGHT_BINARIZATION`, etc.
- ⚠️ Please note that this is the **last release version** which is based on the deprecated Android Support Libraries. We strongly recommend to migrate your project to AndroidX libs and upgrade to the latest version of the Scanbot SDK (see above) to benefit from the latest features, improvements, bug fixes, etc.

### Version 1.39.52 (18 Jun 2019)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.52.0, but **without AndroidX** dependencies.

### Version 1.39.51 (6 Jun 2019)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.51.0, but **without AndroidX** dependencies.

### Version 1.39.50 (10 May 2019)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.50.0, but **without AndroidX** dependencies.

### Version 1.39.1 (15 Feb 2019)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.41.0, but **without AndroidX** dependencies.

### Version 1.39.0 (19 Dec 2018)
- A special release for our customers who are not yet able to switch their projects to AndroidX. This version provides the same features, improvements and fixes as version 1.40.0, but **without AndroidX** dependencies.

### Version 1.38.3 (19 Nov 2018)
- 🐞 Bug fixes:
  * Small fixes in `MRZScanner` (improved detection on still images of some ID cards)

### Version 1.38.2 (25 Oct 2018)
- 🚀 Improvements:
  * Updated Google Mobile Vision lib version to 16.2.0

### Version 1.38.1 (16 Oct 2018)
- 🐞 Bug fixes:
  * Fixed a bug with freezing camera on Activity start
  * Fixed "CroppingActivity" layout for "ready-to-use-ui".

### Version 1.38.0 (11 Oct 2018)
- 🚀 Improvements:
  * Updated jpeg-turbo lib version to 1.5.3

### Version 1.37.0 (27 Sep 2018)
- 🎉 New:
  * Reset/Detect functionality in RTU Cropping UI
  * Added support for `orientationLockMode` in RTU Cropping UI
- ⚠️ Breaking changes:
  * The config parameter `pageCounterButtonTitle` in RTU Document Scanner UI now requires a placeholder "%d" for the number of pages (e.g. `pageCounterButtonTitle: "%d Page(s)"`)
- 🐞 Bug fixes:
  * Fixed an issue with camera on "Xiaomi Redmi 5 Plus" devices with MIUI Chinese ROM ([#72](https://github.com/doo/scanbot-sdk-example-android/issues/72))

### Version 1.36.0 (19 Sep 2018)
- 🚀 Improvements:
  * MRZ Recognizer: improved detection on still images and parsing of some optional MRZ fields.
  * Check out the updated [mrz-scanner](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/mrz-scanner) and [ready-to-use-ui-demo](https://github.com/doo/scanbot-sdk-example-android/tree/master/ready-to-use-ui-demo) example apps.
- ⚠️ Breaking changes:
  * All MRZ Scanner and Recognizer components (`MRZScanner`, `MRZScannerActivity`) now require an additional trained data blob file (MRZ cascade blob file `mrz.xml`), which is included in the SDK `io.scanbot:sdk-package-3` and can be fetched via `blobManager.fetch(blobFactory.mrzCascadeBlob(), false)`!

### Version 1.35.0 (10 Sep 2018)
- 🎉 New:
  * Cheque Scanner - Real-time extraction of account & routing number (check out the [cheque-scanner](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/cheque-scanner) example app)
  * Added a new config parameter `rotateButtonHidden` for the **RTU UI** `CroppingActivity`
- ⚠️ Breaking change: Added file format extension (.jpg or .png) for **RTU UI** `Page` images:
  * Affects the image files created by all **RTU UI** components, like `DocumentScannerActivity`, `CroppingActivity`, etc).
  * Please note that only the **new created** image files will contain extensions. The **currently available** image files in the temporary storage of the Scanbot SDK
    **will not get** file extensions and may become inaccessible. So please make sure to implement a suitable migration mechanism.
  * If you need backwards compatibility, you can disable the file format extensions via `PageStorageSettings.addImageFileFormatExtension(false)` on initialization of the SDK (`new ScanbotSDKInitializer().usePageStorageSettings(new PageStorageSettings.Builder().addImageFileFormatExtension(false).build())...`).
- 🐞 Bug fixes:
  * MRZ Recognizer: fixed extraction of the field `dateOfBirth` from some French ID cards
- 🚀 Improvements:
  * Updated Google Vision lib version to 15.0.2
  * Improvements and potential fixes in SDK license manager

### Version 1.34.0 (28 Aug 2018)
* Updated OpenCV version to 3.4.2
* DC Scanner:
  - Improved recognition of low-constrast scans
  - ⚠️ Breaking change: The DC Scanner now uses `eng.traineddata` instead of `deu.traineddata`!

### Version 1.33.3 (19 Jul 2018)
* Removed `allowBackup` flag in AndroidManifest.xml in `io.scanbot:sdk-package-ui` library

### Version 1.33.2 (17 Jul 2018)
* Minor fixes and improvements in `EditPolygonImageView`

### Version 1.33.1 (9 Jul 2018)
* DC scanner: improved date recognition in cases where the date text overlaps the date field's grid
* Minor fixes and improvements in `EditPolygonView`

### Version 1.33.0 (2 Jul 2018)
* Extended and improved the API to apply image filter on pages coming from the Ready-To-Use UI.
* Added new image filters:
  - `IMAGE_FILTER_BACKGROUND_CLEAN` - Cleans up the background and tries to preserve photos within the image.
  - `IMAGE_FILTER_BLACK_AND_WHITE` - Black and white filter with background cleaning. Creates a grayscaled 8-bit image with mostly black or white pixels.
* Minor bug fixes and improvements.

### Version 1.31.3 (22 Jun 2018)
* Several fixes in `io.scanbot.sdk.ScanbotSDKInitializer` and `io.scanbot.sdk.ScanbotSDK`

### Version 1.31.2 (21 Jun 2018)
* Several fixes in `io.scanbot.sdk.ScanbotSDKInitializer` and `io.scanbot.sdk.ScanbotSDK`

### Version 1.31.1 (18 Jun 2018)
* Added functionality to apply a real orientation lock in `ScanbotCameraView`. Use the new methods `cameraView.lockToLandscape(boolean lockPicture)` or `cameraView.lockToPortrait(boolean lockPicture)` to lock the UI as well as the taken picture to a desired orientation.
* Fixed a bug with shutter sound in `ScanbotCameraView` on some Android devices (e.g. Xiaomi).
* Fixed a bug with freezing camera on Huawei P20 Lite.

### Version 1.31.0 (11 Jun 2018)
* Added native libs for `arm64-v8a`.
  * ⚠️ **Please note:** In August 2019, Google Play Store will require that new apps and app updates with native libraries provide 64-bit versions in addition to their 32-bit versions. (https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)
  * We strongly recommend to upgrade the Scanbot SDK for Android to the latest version to benefit from the full support of `arm64-v8a` libs.
* Upgraded Dagger to version 2.16. It is recommended to upgrade the Dagger version in your App to 2.14.x or higher.

### Version 1.30.0 (7 Jun 2018)
* 🎉 New! Added **Ready-To-Use UI** Components: `DocumentScannerActivity`, `CroppingActivity`, `MRZScannerActivity`, `BarcodeScannerActivity`. A set of easy to integrate and customize high-level UI components for the most common tasks in Scanbot SDK.
  * Published a new package `io.scanbot:sdk-package-ui` containing the new **Ready-To-Use UI** Components.
  * Added a new demo project `ready-to-use-ui` containing example code for the new **Ready-To-Use UI** Components.
* Added a beautiful, animated ShutterButton component (`io.scanbot.sdk.ui.camera.ShutterButton`) which can also be used with our **Classical UI Components**. Check out the [camera-view](https://github.com/doo/scanbot-sdk-example-android/tree/master/classical-components-demo/camera-view) example app.
* Minor bug fixes and improvements.
* Internal refactorings and improvements:
  * Deprecated some classes in the `net.doo.snap` Java package. Please use the equivalent classes from the new Java package `io.scanbot.sdk`.
    (e.g. `net.doo.snap.ScanbotSDKInitializer` => `io.scanbot.sdk.ScanbotSDKInitializer`,  `net.doo.snap.ScanbotSDK` => `io.scanbot.sdk.ScanbotSDK`, etc.)

### Version 1.28.7
* Added `setIgnoreBadAspectRatio(boolean ignoreBadAspectRatio)` method for the `AutoSnappingController`.
* Improved `AutoSnappingController` precision.

### Version 1.28.6
* Updated Google Mobile Vision library version to 12.0.1.
* Optimized `ScanbotCameraView` performance.

### Version 1.28.5
* Added new attribute `magnifierEnableBounding` to `MagnifierView`, which allowing to enable/disable bounding of `MagnifierView` to the edge of screen.
* Fixed issue with paddings on a `MagnifierView`

### Version 1.28.4
* `DCScanner` detection quality improvements.

### Version 1.28.3
* Scanbot SDK could be reinitialised with the new SDK license.
* `MagnifierView` should not be the same size as `EditPolygonImageView`, but have to have the same aspect ratio.

### Version 1.28.2
* Removed deprecated attributes `magnifierCrossSize`, `magnifierCrossStrokeWidth`, `magnifierStrokeWidth` and `editPolygonMagnifier` from `MagnifierView` since they don't have any effects on the view.
To customize the `MagnifierView` please use the attributes `magnifierImageSrc`, `magnifierRadius` and `magnifierMargin`.

### Version 1.28.1
* Exposed the method `setImageRotation()` in `MagnifierView`.

### Version 1.28.0
* Added confidence values for detected fields in `MRZScanner`.
* Changed default barcode detector to ZXing barcode detector.
* Added Finder feature for barcode detectors.

### Version 1.27.3
* Updated Google Mobile Vision library version to 12.0.0.

### Version 1.27.2
* Fixed native libraries loading.
* Fixed `DCScanner` bugs.

### Version 1.27.1
* Added `TIFFWriter` feature to the Scanbot SDK package 1.
* Added new "Pure Binarization" (`ContourDetector.IMAGE_FILTER_PURE_BINARIZED`) filter.
* Added `DCScanner` recognition methods for `Bitmap`s and JPEG images.

### Version 1.27.0
* Added `MRZScanner` (Machine Readable Zone scanner) feature to the Scanbot SDK package 3.
* Added Finder feature for `MRZScanner`.
* Added `DCScanner` (Disability Certificate scanner) feature to the Scanbot SDK package 4.
* Updated Tesseract version to 3.05.01.
* Updated Android support library dependencies version to 27.0.2.

### Version 1.26.7
* Minor bug fixes and improvements

### Version 1.26.6
* Minor bug fixes

### Version 1.26.5
* Added `cameraView.setPreviewMode(CameraPreviewMode mode)` for adjusting camera preview frames in the camera layout.
* Minor bug fixes

### Version 1.26.4
* Minor bug fixes

### Version 1.26.2
* Fixed bug when `cameraView.setShutterSound(boolean enable)` throws RuntimeException if the camera is already released.

### Version 1.26.1
* Updated transitive Scanbot SDK dependencies
* Added `cameraView.setShutterSound(boolean enable)` and `cameraView.setAutoFocusSound(boolean enable)` methods in `ScanbotCameraView`
* Optimized `AutoSnappingController` sensitivity and auto snapping timings

### Version 1.26.0
* Optimized Scanbot SDK license check - added Java exceptions with human readable messages
* Added `ScanbotSDK#isLicenseValid()` method for the license validation
* Fixed bug when the image preview edge zooming in `MagnifierView` was displayed incorrectly

### Version 1.25.2
* Added detected text paragraphs, lines and words blocks of the optical character recognitions result in `OcrResult`
* Added `PageFactory#buildPage(Bitmap image)` and `PageFactory#buildPage(byte[] image)` methods for `Page` generation

### Version 1.25.1
* Added rotation methods for `EditPolygonImageView`

### Version 1.25.0
* Added Barcode/QR code scanner feature to the Scanbot SDK package 1

### Version 1.24.0
* Optimized transitive dependencies for Scanbot SDK library
* Stop supporting `armeabi` architecture

### Version 1.23.3
* Fixed contour detection and autosnapping freeze on the autofocus tap

### Version 1.23.2
* Added `detectionScore` value in `DetectedFrame` class
* Fixed 180 degree camera preview rotation

### Version 1.23.0
* Added Scanbot SDK package 3 with SEPA Pay Form scanner feature

### Version 1.22.6
* Fixed bug when `ScanbotCameraView` crashes with `IllegalStateException` after `onPause`

### Version 1.22.5
* Added methods for setting contour detector parameters in `ContourDetectorFrameHandler`

### Version 1.22.4
* Added methods for setting OCR and language classifier blobs paths in `ScanbotSDKInitializer`
* Updated version of transitive dependencies

### Version 1.22.3
* Added continuous focus option for `ScanbotCameraView`

### Version 1.22.2
* Added method that provides internal OCR blobs directory in `BlobManager` in SDK-2

### Version 1.22.1
* Added methods for performing OCR with multiple predefined languages in `TextRecognition` in SDK-2

### Version 1.22.0
* Scanbot SDK was switched from RoboGuice DI to Dagger 2

### Version 1.21.3
* Fixed minor camera issues
* Added setters for edge width and color in `EditPolygonImageView`

### Version 1.21.2
* Fixed dependencies in SDK packages

### Version 1.21.1
* Added camera preview and picture size setters in `ScanbotCameraView`
* Added camera orientation locks in `ScanbotCameraView`

### Version 1.19.0
* Fixed issue when `EditPolygonImageView` with `MagnifierView` was working only as part of an `Activity`.
* Removed `DrawMagnifierListener`.

### Version 1.18.4
* `EditPolygonImageView` was not working properly on Android Nougat.

### Version 1.18.2
* It was not possible to create PDFs without embedded text in SDK-2

### Version 1.18.1
* Fixed crash related to text recognition

### Version 1.18.0
* It's now possible to perform OCR without sandwiched PDF as part of result

### Version 1.17.0
* It's now possible to customize the `Logger` implementation used by the SDK.

### Version 1.16.0
* Added `ContourDetector.processImageAndRelease` - more memory efficient version of `ContourDetector.processImageF`.

### Version 1.15.3
* Fixed build issues of OCR example.

### Version 1.15.1
* Removed uses-feature android.hardware.camera. Camera is now optional.

### Version 1.14.0
* Removed unused dependencies. Removed permission declarations from Package 1. Users are now responsible for declaring permissions. For more information see [this page](https://github.com/doo/Scanbot-SDK-Examples/wiki/Permissions).

### Version 1.13.1
* Fixed bug when final image contained less than preview image from camera stream.

### Version 1.13.0
* Added `ImageQualityOptimizer` which allows you to optimize image size.

### Version 1.12.2
* Fixed bug when `AutosnappingController` stop operating after `onPause`.

### Version 1.12.1
* Color-document filter was disabled in `ContourDetector`. Now it works again.

### Version 1.12.0
* Added `ScanbotSDKInitializer#withLogging()` method which allows you to turn on logging for SDK (disabled by default).
* Improved edge detection.
* Added new filter to `ContourDetector`.

### Version 1.11.0
* Added `ScanbotSDK.isLicenseActive()` method.
