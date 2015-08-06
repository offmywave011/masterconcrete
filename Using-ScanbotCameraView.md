Although Scanbot SDK provides out-of-the-box UI components such as `ScanbotCameraFragment` and `SnappingActivity`, they are rather limited in terms of customization. If you found yourself in a such situation then you should use camera directly.

Android camera API might be very tricky and far from being developer-friendly (in fact, very far). To help you skip same issues as we encountered while developing Scanbot we created `ScanbotCameraView`.

#### Getting started

`ScanbotCameraView` is available with SDK Package 1. To get started with it you have to do 2 steps.

First. Add it to your layout, which is as simple as:

    <net.doo.snap.camera.ScanbotCameraView
            android:id="@+id/camera"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

Second. Delegate `onResume` and `onPause` methods of your `Activity` (or `Fragment`, whatever you are using) to `ScanbotCameraView`:

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