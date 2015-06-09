We set up our project, so now we can actually start doing something useful.

## How it works

SDK works in 2 steps:

1. Scan the document. This step will output array of `DocumentDraft` objects. Each of them represents a document which user wants to create.
2. Process each `DocumentDraft`. This will give you the final result (saved document) as a `File`.

## Scanning

To start scanning you should simply start `SnappingActivity` which will return you the result in form of `DocumentDraft[]`

    startActivityForResult(
        new Intent(v.getContext(), SnappingActivity.class),
        REQUEST_CODE    // not specific to our SDK - see common usage of startActivityForResult
    );

This will launch snapping screen where user will be able to take scans, crop/rotate images and apply various filters. After user will hit Save button, you'll get the result back:

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode != REQUEST_CODE || resultCode != RESULT_OK) {
            return;
        }

        Parcelable[] parcelableArray = data.getParcelableArrayExtra(Constants.SNAPPING_RESULT);
        if (parcelableArray == null) {
            return;
        }

        DocumentDraft[] documentDrafts = Arrays.copyOf(parcelableArray, parcelableArray.length, DocumentDraft[].class);

        // Now we are ready to start processing
    }

Each `DocumentDraft` represents a template for a new document. But document itself is not created yet - each draft should be processed by `DocumentProcessor` before you'll get actual files.

**Note:** you don't have to start processing instantly. For instance, you can persist `DocumentDraft` and perform processing at some point in future.

## Processing

Now, when we have our `documentDrafts` creating the documents is as simple as:

    for (DocumentDraft draft : documentDrafts) {
        DocumentProcessingResult result = documentProcessor.processDocument(draft);

        // handle the result, such as result.getDocumentFile()
    }

`DocumentProcessing` will do all the processing and write the documents to the filesystem. You can use `DocumentProcessingResult.getDocumentFile()` to open the file afterwards.

**Note:** `processDocument` is blocking, CPU and memory intensive operation, so you should not call it from application's main thread. Our recommended practice is to perform processing in `IntentService`, so processing won't be stopped when user will leave your app.