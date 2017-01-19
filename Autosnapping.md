To further improve the user experience, you might want to automatically take a photo when a document is detected and conditions are good - we call this Autosnapping.

#### How to use it?

It is easy: just attach `AutoSnappingController` to the camera like in the following example:

        ContourDetectorFrameHandler contourDetectorFrameHandler = ContourDetectorFrameHandler.attach(cameraView);
       
        contourDetectorFrameHandler.addResultHandler(polygonView); // optional

        AutoSnappingController.attach(cameraView, contourDetectorFrameHandler);

And you're done. Now the camera will automatically take photos when the underlying conditions are perfect.