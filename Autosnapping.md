To further improve user experience you might want to automatically take a photo when document is detected and conditions are good - we call this Autosnapping.

#### How to use it?

Easy, just attach `AutoSnappingController` to the camera like following:

        ContourDetectorFrameHandler contourDetectorFrameHandler = ContourDetectorFrameHandler.attach(cameraView);
       
        contourDetectorFrameHandler.addResultHandler(polygonView); // optional

        AutoSnappingController.attach(cameraView, contourDetectorFrameHandler);

And you're done. Now camera will automatically take photos when conditions are perfect.