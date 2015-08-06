After you set up `ScanbotCameraView` next logical step would be to start using contour detection and draw results on screen.

#### Some background

`ScanbotCameraView` has a method `getPreviewBuffer()` which allows you to register for preview frames from the camera. While you can implement your own smart features Scanbot SDK comes with built-in `ContourDetectorFrameHandler` which performs contour detection and outputs results to listeners.

#### Contour detection

To start contour detection, you have to attach `ContourDetectorFrameHandler` to preview buffer:

    ScanbotCameraView cameraView = (ScanbotCameraView) findViewById(R.id.cameraView);

    ContourDetectorFrameHandler frameHandler = new ContourDetectorFrameHandler((Context) this);
    cameraView.getPreviewBuffer().addFrameHandler(frameHandler);

or even shorter

    ContourDetectorFrameHandler frameHandler = ContourDetectorFrameHandler.attach(cameraView);

At his point contour detection becomes active. Now, all we have to do is listen for results:

    frameHandler.addResultHandler(new ContourDetectorFrameHandler.ResultHandler() {

        @Override
        public boolean handleResult(ContourDetectorFrameHandler.DetectedFrame result) {
            // Handle result as you like
            return false;
        }

    });