The Android camera API might be very tricky and far from being developer-friendly (in fact, very far). To help you skip the same issues we encountered while developing Scanbot, we created `ScanbotCameraView`.

#### Getting started

`ScanbotCameraView` is available with the SDK Package 1. To get started, you have to undertake 3 steps.

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