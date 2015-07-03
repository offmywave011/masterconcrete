If you want to use only camera functionality without ViewPager, color filters or page cropping you can just embed `ScanbotCameraFragment` into your `Activity`.

However, there are some extra steps which you need to take:

#### Add ScanbotCameraFragment

Just add it as any usual fragment. Either programmatically, or directly in layout XML definition:

    <fragment 
        android:id="@+id/camera"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        class="net.doo.snap.ui.camera.ScanbotCameraFragment" />

#### Use/Extend ScanbotTheme

`ScanbotCameraFragment` relies on properties set in `ScanbotTheme`. So, your activity must either use this theme or extend from it:

    <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:theme="@style/ScanbotTheme" />

#### Use ActionBar overlay mode

Inside your `onCreate` method in `Activity`, call this:

    supportRequestWindowFeature(WindowCompat.FEATURE_ACTION_BAR_OVERLAY);

#### Handling the result

Your activity have to implement `ScanbotCameraFragment.PictureTakenListener` interface. You will get an image byte array and image orientation as `onPictureTaken(byte[] image, int imageOrientation)` method parameters. They are the result of camera snap or image selection from Gallery:

    public class ExampleActivity extends RoboActionBarActivity implements ScanbotCameraFragment.PictureTakenListener     
    {
        @Override
        public void onPictureTaken(byte[] image, int imageOrientation) {
            Toast.makeText(this, "Picture delivered!", Toast.LENGTH_SHORT).show();
            //now you can save and start picture processing here (e.g. applying filters, cropping)
        }
    }

`byte[] image` - full size not cropped raw image from camera.