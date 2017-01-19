## Background

In the Scanbot SDK we have a notion of `SnappingDraft` and `DocumentDraft`.

`SnappingDraft` is a direct result of snapping - a set of files, pages and their properties. It is not yet a complete logical document. In fact - you might split such a draft into several documents!

Therefore, before we can start processing, we should extract the `DocumentDraft` from the `SnappingDraft`. Each `DocumentDraft` will represent a logical document which will be created including information about the document's name and file format.

## Extracting DocumentDrafts

The logic for splitting `SnappingDraft` into a set of `DocumentDraft` objects is the responsibility of the `DocumentDraftExtractor`. You can customize it through the `ScanbotSDKInitializer`. The default implementation saves a document as a single PDF file.

For your convenience we created an implementation of the extractor which always saves a document as a set of JPEG images. You just have to set it up as follows:

    new ScanbotSDKInitializer()
            .documentDraftExtractor(MultipleDocumentsDraftExtractor.forJpeg())
            .initialize(this);