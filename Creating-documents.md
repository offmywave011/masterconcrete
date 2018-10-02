We have set up our project, so now we can actually start doing something useful.

## How it works

The document creation workflow:

1. Scan the page to obtain `Bitmap` (see `ScanbotCameraView`).
1. (Optionally) Crop `Bitmap` and apply filters (see `ContourDetector`)
1. Create `Page` from `Bitmap`. Accumulate pages in `SnappingDraft` object.
1. Convert pages stored in `SnappingDraft` into documents using `DocumentDraftExtractor`. This step gives you the `DocumentDraft` items
1. Process each `DocumentDraft`. This will give you the final result (saved document) as a `File`.
1. Clean up

#### Step 0 - Set up

Add these permissions to your AndroidManifest.xml

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

Create a `ScanbotSDK` object in your `Activity` or `Service`:

    private ScanbotSDK scanbotSDK;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // ...

        scanbotSDK = new ScanbotSDK(this);
    }

You will use this object to access the SDK's features.

#### Step 1 - Scanning

This topic is covered here: [[Using ScanbotCameraView]]

#### Step 2 - Cropping and filtering

This topic is covered here: [[Advanced: Detection and Filters]]

#### Step 3 - Create `Page`

The `Page` object represents the snapped page without holding the reference to the actual `Bitmap` (which is written to the file). This allows you to keep a lot of pages in the memory.

    PageFactory pageFactory = scanbotSDK.pageFactory();
    Page page = pageFactory.buildPage(result, screenWidth, screenHeight).page;

Now, when you have a page, you can add it to `SnappingDraft`. This object represents the draft that will become a document (or even multiple documents) in the future. Right now it is just an ordered collection of pages with some meta-information (e.g. names).

    SnappingDraft draft = new SnappingDraft(page);

**Note:** you can create a draft without pages in it and add pages later.

#### Step 4 - Convert `SnappingDraft` to one/many `DocumentDraft`

`SnappingDraft` represents a single document or can be arbitrarily split into several documents. This is where `DocumentDraftExtractor` kicks in. It's responsibility is to split `SnappingDraft` into one and/or many `DocumentDraft`:

    DocumentDraftExtractor documentDraftExtractor = scanbotSDK.documentDraftExtractor();
    DocumentDraft[] drafts = documentDraftExtractor.extract(snappingDraft);

Each `DocumentDraft` represents a template for a new document. Anyhow, the document itself is not created yet - each draft should be processed by `DocumentProcessor` before you will get the actual files.

`scanbotSDK.documentDraftExtractor()` returns default `DocumentDraftExctactor` which treats every `snappingDraft` as single document. You can change this behaviour by either implementing your own extractor or using one which is coming with the SDK (see API docs of `MultipleDocumentsDraftExtractor`).

**Note:** you don't have to start processing instantly. For instance, you can persist `DocumentDraft` and perform processing at some point in the future.

#### Step 5 - Processing

Now when we have our `documentDrafts`, creating the documents is as simple as:

    for (DocumentDraft draft : documentDrafts) {
        DocumentProcessingResult result = documentProcessor.processDocument(draft);

        // handle the result, such as result.getDocumentFile()
    }

`DocumentProcessing` will do all the processing and write the documents to the filesystem. You can use `DocumentProcessingResult.getDocumentFile()` to open the file afterwards.

**Note:** `processDocument` is blocking, CPU and memory intensive operation, so you should not call it from the application's main thread. Our recommended practice is to perform processing in `IntentService`, so processing will not be stopped when a user will leave your app.

#### Step 6 - Clean up

During processing, we are leaving several files for each created page. Usually, we do not need them any longer after they have been processed, so we must perform a clean up. To help you with this, we have a `Cleaner` class. Just call it after processing:

    cleaner.cleanup();

And that is it.

**Note:** by default, the `Cleaner` never touches processed documents themselves. If that is not what you want, you can specify your implementation of `UnreferencedSourcesProvider` in `ScanbotSDKInitializer`