## Background

In Scanbot SDK we have a notions of `SnappingDraft` and `DocumentDraft`.

`SnappingDraft` is a direct result of snapping - set of files, pages and their properties. It is not yet a complete logical document. In fact - you might split such draft into several documents!

Therefore, before we can start processing we should extract `DocumentDraft` from `SnappingDraft`. Each `DocumentDraft` will represent a logical document which will be created, including information about document name and file format.

## Extracting DocumentDrafts

Logic for splitting `SnappingDraft` into set of `DocumentDraft` objects is responsibility of `DocumentDraftExtractor`. You can customize it through `ScanbotSDKInitializer`. Default implementation saves document as a single PDF file.

For your convenience we created implementation of extractor which always saves document as set of JPEG images. You just have to set it up as follows:

    new ScanbotSDKInitializer()
            .documentDraftExtractor(MultipleDocumentsDraftExtractor.forJpeg())
            .initialize(this);