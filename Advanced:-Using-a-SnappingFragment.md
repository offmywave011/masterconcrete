As an alternative to using `SnappingActivity`, you can also embed `SnappingFragment` into your `Activity`.

However, there are some extra steps which you need to take:

#### Add SnappingFragment

Just add it as any usual fragment. Either programmatically, or directly in layout XML definition:

    <fragment
        android:id="@+id/snapping_fragment"
        class="net.doo.snap.ui.SnappingFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

#### Use/Extend ScanbotTheme

`SnappingFragment` relies on properties set in `ScanbotTheme`. So, your activity must either use this theme or extend from it:

    <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:theme="@style/ScanbotTheme" />

#### Use ActionBar overlay mode

Inside your `onCreate` method in `Activity`, call this:

    supportRequestWindowFeature(WindowCompat.FEATURE_ACTION_BAR_OVERLAY);

#### Handling the result

Instead of receiving the result via `onActivityResult`, you'll receive broadcast with action `Constants.SNAPPING_RESULT_ACTION`. So, you have to register your `BroadcastReceiver` in your `Activity`:

    private BroadcastReceiver snappingReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            onSnappingResult(intent);
        }
    };

    @Override
    protected void onResume() {
        super.onResume();
        registerReceiver(snappingReceiver, new IntentFilter(Constants.SNAPPING_RESULT_ACTION));
    }

    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(snappingReceiver);
    }

    private void onSnappingResult(Intent data) {
        // handle the result in the same way as you would do with SnappingActivity
    }