[`ContourDetector`](https://scanbotsdk.github.io/documentation/android/api/net.doo.snap/net/doo/snap/lib/detector/ContourDetector.html) is used for performing document detection and applying the image filters.

## Document Detection

The Document Detection of Scanbot SDK is based on edge detection. To detect the edges and get the boundaries of a document sheet on a still image use the `detect(image)` method of `ContourDetector`:

    ContourDetector detector = new ContourDetector();
    DetectionResult result = detector.detect(origininalBitmap);  // pass a Bitmap or the image data as byte[]
    List<PointF> polygon = detector.getPolygonF();

Here we are detecting the boundaries and receiving a [`DetectionResult`](https://scanbotsdk.github.io/documentation/android/api/net.doo.snap/net/doo/snap/lib/detector/DetectionResult.html). It is an enum which represents the recognition status.

Regardless of the `DetectionResult`, you can acquire the detected boundaries as a polygon using the `detector.getPolygonF()` method. If the polygon was not detected, it will return an empty `List`. On success you will get a `List` with 4 points (one for each corner). Each point has coordinates in a range between [0..1], representing position relative to the image size. For instance, if a point has the coordinates (0.5, 0.5), it means that it is located exactly in the center of the image.

## Cropping and Applying Image Filter

To crop the document image from the original image based on detected polygon use one of the `processImage(..)` methods of the `ContourDetector`:

    Bitmap documentBitmap = detector.processImageF(origninalBitmap, polygon, imageFilter);

The parameters are:
* `originalBitmap` - The original `Bitmap` image as input to crop the document image from. This input image will **not** be modified.
* `polygon` - Detected boundaries as polygon `List` (`detector.getPolygonF()`) or an empty `List` if cropping is not desired and you just want to apply an image filter.
* `imageFilter` - Code of the optional image filter to apply.

The result `documentBitmap` is the cropped (and optionally filtered) document image.

Constants for supported image filters:
* `ContourDetector.IMAGE_FILTER_NONE` - Don't apply an image filter, keep original colors.
* `ContourDetector.IMAGE_FILTER_COLOR_ENHANCED` - Optimizes colors, contrast and brightness.
* `ContourDetector.IMAGE_FILTER_GRAY` - Grayscale filter.
* `ContourDetector.IMAGE_FILTER_BINARIZED` - Standard binarization filter with contrast optimization. Creates a grayscaled 8-bit image with mostly black or white pixels.
* `ContourDetector.IMAGE_FILTER_COLOR_DOCUMENT` - MagicColor filter. Fixes white-balance and cleans up the background.
* `ContourDetector.IMAGE_FILTER_PURE_BINARIZED` - A filter for binarizing an image. Creates an image with pixel values set to either pure black or pure white.
* `ContourDetector.IMAGE_FILTER_BLACK_AND_WHITE` - Black and white filter with background cleaning. Creates a grayscaled 8-bit image with mostly black or white pixels.
* `ContourDetector.IMAGE_FILTER_BACKGROUND_CLEAN` - Cleans up the background and tries to preserve photos within the image.
