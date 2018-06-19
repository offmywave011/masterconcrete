Once a picture has been taken, whether automatically by the autosnapping feature or manually by user, you have to handle the image data by implementing the method `public void onPictureTaken(byte[] image, int imageOrientation)` of the `PictureCallback` interface.
In this method you receive the image byte array of the original picture data and the image orientation value.

It is important to understand that this image data represents the **original picture** and not the cropped document image.

To get the cropped document image, you have to perform document contour detection on it and apply the cropping operation by using the `ContourDetector` class:


```
@Override
public void onPictureTaken(byte[] image, int imageOrientation) {
    // Decode image byte array to Bitmap:
    Bitmap originalBitmap = BitmapFactory.decodeByteArray(image, 0, image.length);

    // Rotate the original image if required:
    if (imageOrientation > 0) {
        Matrix matrix = new Matrix();
        matrix.setRotate(imageOrientation, originalBitmap.getWidth() / 2f, originalBitmap.getHeight() / 2f);
        originalBitmap = Bitmap.createBitmap(originalBitmap, 0, 0, originalBitmap.getWidth(), originalBitmap.getHeight(), matrix, false);
    }

    // Run document contour detection on original image:
    ContourDetector detector = new ContourDetector();
    detector.detect(originalBitmap);
    // And crop by detected polygon to get the final document image:
    Bitmap documentImage = detector.processImageAndRelease(originalBitmap, detector.getPolygonF(), ContourDetector.IMAGE_FILTER_NONE);

    // Work with the final document image (store it as file, etc)
    // ...

    // Continue with the camera preview to scan the next image:
    cameraView.post(new Runnable() {
        @Override
        public void run() {
            cameraView.continuousFocus();
            cameraView.startPreview();
        }
    });
}
```

## Handling the `imageOrientation` parameter
TODO ...