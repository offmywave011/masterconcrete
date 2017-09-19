`ContourDetector` is used for performing document contour detection and applying the image filters.

## Detecting bounds

To detect the bounds, you can use this snippet:

    ContourDetector detector = new ContourDetector();
    DetectionResult result = detector.detect(image);  // use Bitmap or byte[]
    List<PointF> polygon = detector.getPolygonF();

We are still working on a documentation and possibly nicer wrapper for our `ContourDetector`, so it might be not that obvious what is going on.

First, we are detecting the boundaries of the image and receiving a `DetectionResult`. It is an enum which represents the recognition status - whether it was good or not and how the user can improve it (by moving the camera closer, rotating the device, etc.).

Regardless of the `DetectionResult`, you can acquire the detected polygon using the `detector.getPolygonF()` method. If the polygon was not detected, it will output an empty `List`. On success you will get a `List` with 4 points (one for each corner). Each point has coordinates in a range between [0..1], representing position relative to the image size. For instance, if a point has the coordinates (0.5, 0.5), it means that it is located exactly in the center of the image.

## Applying filter and cropping

Here is the snippet for applying the filters and perform cropping:

    Bitmap result = detector.processImageF(origninalBitmap, polygon, imageFilter);

The parameters are:
* `originalBitmap` - `Bitmap` which will be modified.
* `polygon` - `List` from `detector.getPolygonF()` or empty `List` if cropping is not desired.
* `imageFilter` - code of the image filter.

Supported image filters:
* `IMAGE_FILTER_NONE` - don't use image filter, keep original colors
* `IMAGE_FILTER_COLOR_ENHANCED` - color-enhancement filter
* `IMAGE_FILTER_GRAY` - grayscale filter
* `IMAGE_FILTER_BINARIZED` - black&white filter
* `IMAGE_FILTER_COLOR_DOCUMENT` - colored document image filter