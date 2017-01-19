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

Done. If you'll start the app, the polygon will be drawn on the screen.

#### Customizing drawn polygon

`PolygonView` supports the following attributes (which you can add in XML, as shown in the example above):

* `polygonStrokeWidth` - width (thickness) of polygon lines
* `polygonStrokeColor` - color of polygon lines
* `polygonFillColor` - fill color of polygon