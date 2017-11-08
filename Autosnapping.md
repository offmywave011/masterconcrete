To further improve the user experience, you might want to automatically take a photo when a document is detected and conditions are good - we call this Autosnapping.

#### How to use it?

It is easy: just attach `AutoSnappingController` to the camera like in the following example:

        ContourDetectorFrameHandler contourDetectorFrameHandler = ContourDetectorFrameHandler.attach(cameraView);
       
        contourDetectorFrameHandler.addResultHandler(polygonView); // optional

        AutoSnappingController.attach(cameraView, contourDetectorFrameHandler);

And you're done. Now the camera will automatically take photos when the underlying conditions are perfect.

#### Sensitivity

You can control auto-snapping speed by setting `sensitivity` parameter in `AutoSnappingController`. 

        autoSnappingController.setSensitivity(1f);

That is: the more sensitive it is the faster it shoots. Sensitivity must be within [0..1] range. A value of 1.0 triggers automatic capturing immediately, a value of 0.0 delays the automatic by 3 seconds.

The default value is 0.66 (1 sec)

