Since version **1.40.0** the Scanbot SDK for Android provides a simple API ([`PDFRenderer`](https://scanbotsdk.github.io/documentation/android/api/io.scanbot.sdk/io/scanbot/sdk/process/PDFRenderer.html)) to create multipage PDF files. Each image will be stored as a PDF page.

Get an instance of `PDFRenderer` from `ScanbotSDK`:
```
import io.scanbot.sdk.ScanbotSDK;
import io.scanbot.sdk.process.PDFRenderer;
import io.scanbot.sdk.process.PDFPageSize;

PDFRenderer pdfRenderer = new ScanbotSDK(this).pdfRenderer();
```

Then you can create a PDF file from `Page` objects stored by our **RTU UI** components (e.g. result from the `DocumentScannerActivity`):
```
List<io.scanbot.sdk.persistence.Page> pages = ...;
File pdfFile = pdfRenderer.renderDocumentFromPages(pages, PDFPageSize.FIXED_A4);
```
Please note: The PDF Renderer uses the **document image** (cropped image) of a `Page` object. So make sure all `Page` objects contain document images.


Or alternatively create a PDF file from arbitrary image files (JPG or PNG) provided as file URIs:
```
List<android.net.Uri> imageFileUris = ...;  // ["file:///some/path/file1.jpg", "file:///some/path/file2.jpg", ...]
File pdfFile = pdfRenderer.renderDocumentFromImages(imageFileUris, PDFPageSize.FIXED_A4);
```

You can specify one of the following [`PDFPageSize`s](https://scanbotsdk.github.io/documentation/android/api/io.scanbot.sdk/io/scanbot/sdk/process/PDFPageSize.html) as output size:
- `A4`: The page has the aspect ratio of the image, but is fitted into A4 size. Whether portrait or landscape depends on the images aspect ratio.
- `FIXED_A4`: The page has A4 size. The image is fitted and centered within the page. Whether portrait or landscape depends on the images aspect ratio.
- `US_LETTER`: The page has the aspect ratio of the image, but is fitted into US letter size. Whether portrait or landscape depends on the images aspect ratio.
- `FIXED_US_LETTER`: The page has US letter size. The image is fitted and centered within the page. Whether portrait or landscape depends on the images aspect ratio.
- `AUTO`: For each page the best matching format (A4 or US letter) is used. Whether portrait or landscape depends on the images aspect ratio.
- `AUTO_LOCALE`: Each page of the result PDF will be of US letter or A4 size depending on the current locale. Whether portrait or landscape depends on the images aspect ratio.
- `FROM_IMAGE`: Each page is as large as its image at 72 dpi.
