You can handle the contour detection results using `ContourDetectorFrameHandler#addResultHandler`. It might be useful if you want to guide your user through the snapping process by, for instance, displaying respective icons and status messages.

    contourDetectorFrameHandler.addResultHandler(new ContourDetectorFrameHandler.ResultHandler() {

        @Override
        public boolean handleResult(ContourDetectorFrameHandler.DetectedFrame result) {
            // Handle result here
            return false; // typically you need to return false
        }

    }

**Warning:** this callback is coming from the worker thread. You need to move execution to the main thread before updating the UI.

On each frame you will get a `DetectedFrame` object which contains the results of the contour detection. One of the most important fields here is `detectionResult` which is basically the status of the contour detection. Possible values for this status are:

* `OK` - contour detection was successful. Detected contour looks like a valid document. It's good time to take a picture.
* `OK_BUT_TOO_SMALL` - document was detected, but it takes too small area in camera viewport. Quality can be improved by moving camera closer to the document.
* `OK_BUT_BAD_ANGLES` - document was detected, but perspective is wrong (camera is tilted relatively to the document). Quality can be improved by holding camera directly over the document.
* `OK_BUT_BAD_ASPECT_RATIO` - document was detected, but it has wrong rotation relatively to the camera sensor. Quality can be improved by rotating camera by 90 degrees.
* `ERROR_TOO_DARK` - document was not found, most likely because of bad lightning conditions.
* `ERROR_TOO_NOISY` - document was not found, most likely because there is too much background noise (maybe too many other objects on the table, or background texture is not monotonic).
* `ERROR_NOTHING_DETECTED` - document was not found. Probably it is not in viewport. Usually it does not makes sense to show any information to the user at this point.