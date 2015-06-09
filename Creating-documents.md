We set up our project, so now we can actually start doing something useful.

## How it works

SDK works in 2 steps:

1. Scan the document. This step will output array of `DocumentDraft` objects. Each of them represents a document which user want to create.
2. Process each `DocumentDraft`. This will give you the final result (saved document) as a `File`.

