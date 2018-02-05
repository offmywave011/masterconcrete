The Android camera API might seem to be very tricky and far from being developer-friendly (in fact, very far). To help you avoid the same issues which we have encountered while developing Scanbot, we created the `ScanbotCameraView`.

#### Getting started

`ScanbotCameraView` is available with the SDK Package 1. To get started, you have to undertake 3 steps.

First: Add this permission to the AndroidManifest.xml

    <uses-permission android:name="android.permission.CAMERA" />

Second: Add it to your layout, which is as simple as:

    <net.doo.snap.camera.ScanbotCameraView
            android:id="@+id/camera"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

Third: Delegate `onResume` and `onPause` methods of your `Activity` (or `Fragment`, whatever you are using) to `ScanbotCameraView`:

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
            cameraView.stopPreview();
            List<Camera.Size> supportedPictureSizes = cameraView.getSupportedPictureSizes();
            cameraView.setPictureSize(supportedPictureSizes.get(0));
            List<Camera.Size> supportedPreviewSizes = cameraView.getSupportedPreviewSizes();
            cameraView.setPreviewSize(supportedPreviewSizes.get(0));
            cameraView.startPreview();
        }
    });

Also `ScanbotCameraView` supports 2 preview modes:
* `CameraPreviewMode.FIT_IN` - in this mode camera preview frames will be downscaled to the layout view size. Full preview frame content will be visible, but unused edges could be appeared in the preview layout.
* `CameraPreviewMode.FILL_IN` - in this mode camera preview frames fill the layout view. The preview frames may contain additional content on the edges that was not visible in the preview layout.

By default, `ScanbotCameraView` uses `FILL_IN` mode. You can change it using `cameraView.setPreviewMode(CameraPreviewMode mode)` method.

#### Camera auto-focus and shutter sounds

You can enable/disable auto-focus event system and shutter sounds using setters in `ScanbotCameraView`.

    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
            @Override
            public void onCameraOpened() {
                cameraView.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        cameraView.setAutoFocusSound(false);
                        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
                            cameraView.setShutterSound(false);
                        }
                    }
                }, 700);
            }
        });
    
`cameraView.setShutterSound(boolean enabled)` set the camera shutter sound state. By default - `true`, the camera plays the system-defined camera shutter sound when takePicture() is called.

Note that devices may not always allow disabling the camera shutter sound. If the shutter sound state cannot be set to the desired value, this method will be ignored.
https://developer.android.com/reference/android/hardware/Camera.html#enableShutterSound(boolean)

Also, it is supported only with Android API 17+. 

#### Enable continuous focus mode

If you want to enable continuous focus mode you have to call `continuousFocus` method in `ScanbotCameraView`.
This method should be called from the main thread and only when camera is opened.

    cameraView = (ScanbotCameraView) findViewById(R.id.camera);
    cameraView.setCameraOpenCallback(new CameraOpenCallback() {
        @Override
        public void onCameraOpened() {
            cameraView.postDelayed(new Runnable() {
                @Override
                public void run() {
                    cameraView.continuousFocus();
                }
            }, 300);
        }
    });

Continuous focus mode will be automatically disabled after `autoFocus` method call, autoFocus tap on `ScanbotCameraView` or after `takePicture` event. In this cases you have to call `continuousFocus` method again.

    @Override
    public void onPictureTaken(byte[] image, int imageOrientation) {
        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inSampleSize = 8;

        final Bitmap bitmap = BitmapFactory.decodeByteArray(image, 0, image.length, options);

        resultView.post(new Runnable() {
            @Override
            public void run() {
                resultView.setImageBitmap(bitmap);
                cameraView.continuousFocus();
                cameraView.startPreview();
            }
        });
    }