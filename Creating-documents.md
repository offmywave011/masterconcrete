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