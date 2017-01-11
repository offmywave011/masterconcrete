Android camera API might be very tricky and far from being developer-friendly (in fact, very far). To help you skip same issues as we encountered while developing Scanbot we created `ScanbotCameraView`.

#### Getting started

`ScanbotCameraView` is available with SDK Package 1. To get started with it you have to do 3 steps.

First. Add permission to AndroidManifest.xml

    <uses-permission android:name="android.permission.CAMERA" />

Second. Add it to your layout, which is as simple as:

    <net.doo.snap.camera.ScanbotCameraView
            android:id="@+id/camera"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

Third. Delegate `onResume` and `onPause` methods of your `Activity` (or `Fragment`, whatever you are using) to `ScanbotCameraView`:

    public class MyActivity extends Activity {

        // ...

        @Override
        public void onResume() {
            super.onResume();
            scanbotCameraView.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            scanbotCameraView.onPause();
        }

    }

That is it! You can start your app and you should see the camera preview.

#### Modify camera preview and picture size

You can easily change default camera preview and picture sizes using setters in `ScanbotCameraView`.

    cameraView = (ScanbotCameraView) findViewById(R.id.camera);
    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
        @Override
        public void onCameraOpened() {
            List<Camera.Size> supportedPictureSizes = cameraView.getSupportedPictureSizes();
            cameraView.setPictureSize(supportedPictureSizes.get(0));
            List<Camera.Size> supportedPreviewSizes = cameraView.getSupportedPreviewSizes();
            cameraView.setPreviewSize(supportedPreviewSizes.get(0));
        }
    });