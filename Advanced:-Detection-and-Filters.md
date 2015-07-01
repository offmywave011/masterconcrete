`ContourDetector` is used for performing document contour detection and applying image filters.

## Detecting bounds

To detect bounds, you can use this snippet:

    ContourDetector detector = new ContourDetector();
    DetectionResult result = detector.detect(image);  // use Bitmap or byte[]
    List<PointF> polygon = detector.getPolygonF();

We're still working on documentation and possibly nicer wrapper for `ContourDetector`, so it might be not so obvious what is going on.

First, we're detecting the bounds of the image and getting `DetectionResult`. It is an enum which represents recognition status - whether it was good or not and how can user improve it (by moving camera closer, rotating the device, etc.).

Regardless of `DetectionResult`, you can acquire detected polygon using `detector.getPolygonF()` method. If polygon was not detected, it will output empty `List`. On success you'll get `List` with 4 points (one for each corner). Each point has coordinates in range [0..1], representing position relative to image size. For instance, if point has coordinates (0.5, 0.5), it means that it is located exactly in the center of the image.

## Applying filter and cropping

Here is the snippet for applying filters and performing cropping:

    Bitmap result = detector.processImageF(origninalBitmap, polygon, imageFilter);

Parameters are:
* `originalBitmap` - `Bitmap` which will be modified.
* `polygon` - `List` from `detector.getPolygonF()` or empty `List` if cropping is not desired.
* `imageFilter` - code of the image filter.

Supported image filters:
* `IMAGE_FILTER_NONE` - don't use image filter, keep original colors
* `IMAGE_FILTER_COLOR_ENHANCED` - color-enhancement filter
* `IMAGE_FILTER_GRAY` - grayscale filter
* `IMAGE_FILTER_BINARIZED` - black&white filter