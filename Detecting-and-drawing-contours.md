After you have set up the `ScanbotCameraView` the next logical step would be to start using contour detection and draw the results on screen.

#### Some background

`ScanbotCameraView` has a method `getPreviewBuffer()` which allows you to register for preview frames from the camera. While you can implement your own smart features, the Scanbot SDK comes with built-in `ContourDetectorFrameHandler` which performs contour detection and the outputs results to listeners.

#### Contour detection

To start contour detection, you have to attach the `ContourDetectorFrameHandler` to the preview buffer:

    ScanbotCameraView cameraView = (ScanbotCameraView) findViewById(R.id.cameraView);

    ContourDetectorFrameHandler frameHandler = new ContourDetectorFrameHandler((Context) this);
    cameraView.getPreviewBuffer().addFrameHandler(frameHandler);

or even shorter

    ContourDetectorFrameHandler frameHandler = ContourDetectorFrameHandler.attach(cameraView);

At his point, the contour detection becomes active. Now all we have to do is wait for the results:

    frameHandler.addResultHandler(new ContourDetectorFrameHandler.ResultHandler() {

        @Override
        public boolean handleResult(ContourDetectorFrameHandler.DetectedFrame result) {
            // Handle result as you like
            return false;
        }

    });

#### Contour detection parameters

You can easily control the contour detection sensitivity by modifying the optional parameters in `ContourDetectorFrameHandler`:

    ContourDetectorFrameHandler frameHandler = ContourDetectorFrameHandler.attach(cameraView);
    frameHandler.setAcceptedAngleScore(75);
    frameHandler.setAcceptedSizeScore(80);

`setAcceptedAngleScore(Double acceptedAngleScore)` - set the minimum score in percent (0 - 100) of the perspective distortion to accept a detected document. The default value is 75.0. You can set lower values to accept more perspective distortion.

Warning: Lower values result document images which are blurred more.

`setAcceptedSizeScore(Double acceptedSizeScore)` - set the minimum size in percent (0 - 100) of the screen size to accept a detected document. It is sufficient that either the height or the width match the score. The default value is 80.0.

Warning: Lower values result in low resolution document images.

#### Drawing detected contour

To draw the detected contour use `PolygonView`. First, add it as a sub-view of `ScanbotCameraView`:

    <net.doo.snap.camera.ScanbotCameraView
        android:id="@+id/cameraView"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <net.doo.snap.ui.PolygonView
            android:id="@+id/polygonView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:polygonStrokeWidth="8dp"
            app:polygonStrokeColor="#ffffff"
            app:polygonFillColor="#00ff00"/>

    </net.doo.snap.camera.ScanbotCameraView>

Second, `PolygonView` should receive callbacks from `ContourDetectorFrameHandler`:

    PolygonView polygonView = (PolygonView) findViewById(R.id.polygonView);
    frameHandler.addResultHandler(polygonView);

Done. If you are starting the app, the polygon will be drawn on the screen.

#### Customizing drawn polygon

`PolygonView` supports the following attributes (which you can add in XML, as shown in the example above):

* `polygonStrokeWidth` - width (thickness) of polygon lines
* `polygonStrokeColor` - color of polygon lines
* `polygonFillColor` - fill color of polygon