#### Getting started

`EditPolygonImageView` is available with the SDK Package 1. To get started with it, you have to undertake a few steps.

First: Add it to your layout:

    <net.doo.snap.ui.EditPolygonImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:edgeColor="#00cea6"
            app:cornerImageSrc="@drawable/ui_crop_corner_handle"
            app:edgeImageSrc="@drawable/ui_crop_side_handle"
            app:editPolygonHandleSize="48dp"
            app:magneticLineTreshold="10dp" />

Custom parameters `app:editPolygonHandleSize` and `app:magneticLineTreshold` are optional. 

Default polygon for `EditPolygonImageView` could be received from `ContourDetector`.
`ContourDetector` always contains the latest detected contours information like lines and polygons. And after the first 
detection you can set the latest detected contour to `EditPolygonImageView`.

    ContourDetector detector = new ContourDetector();
    DetectionResult detectionResult = detector.detect(image);
    editPolygonView.setPolygon(detector.getPolygonF());

Second: `EditPolygonImageView` supports the magnetic lines feature. You can set the lines like this:

    editPolygonView.setLines(horizontalLinesList, verticalLinesList);

Third: `EditPolygonImageView` supports the magnifying lens feature. To enable it, you should add `net.doo.snap.ui.MagnifierView` to your custom layout.

    <net.doo.snap.ui.MagnifierView
            android:id="@+id/magnifier"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:magnifierImageSrc="@drawable/ui_crop_magnifier"
            app:magnifierCrossSize="16dp"
            app:magnifierCrossStrokeWidth="2dp"
            app:magnifierStrokeWidth="4dp"
            app:magnifierRadius="36dp"
            app:magnifierMargin="16dp"
            app:editPolygonMagnifier="#fff" />

The custom parameters `app:magnifierCrossSize`, `app:magnifierCrossStrokeWidth`, `app:magnifierStrokeWidth`, `app:magnifierRadius`, `app:magnifierMargin` and `app:editPolygonMagnifier` are optional.

**Important** - you should set up the `MagnifierView` every time when editPolygonView is set with a new image:

    magnifierView.setupMagnifier(editPolygonView);

If you want to get a selected polygon from `EditPolygonImageView` just call:

    editPolygonView.getPolygon();


  