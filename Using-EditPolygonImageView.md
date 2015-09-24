#### Getting started

`EditPolygonImageView` is available with SDK Package 1. To get started with it you have to do few steps.

First. Add it to your layout:

    <net.doo.snap.ui.EditPolygonImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:edgeColor="#00cea6"
            app:cornerImageSrc="@drawable/ui_crop_corner_handle"
            app:edgeImageSrc="@drawable/ui_crop_side_handle"
            app:editPolygonHandleSize="48dp"
            app:magneticLineTreshold="10dp" />

Custom parameters `app:editPolygonHandleSize` and `app:magneticLineTreshold` are optional. 

Second. `EditPolygonImageView` supports magnetic lines feature. You can set lines this way:

    editPolygonView.setLines(horizontalLinesList, verticalLinesList);

Third. `EditPolygonImageView` supports magnifying lens feature. To enable it you should add `net.doo.snap.ui.MagnifierView` to your layout.

    <net.doo.snap.ui.MagnifierView
            android:id="@+id/magnifier"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:magnifierImageSrc="@drawable/ui_crop_magnifier"
            app:magnifierCrossSize="16dp"
            app:magnifierCrossStrokeWidth="2dp"
            app:magnifierStrokeWidth="4dp"
            app:magnifierRadius="36dp"
            app:magnifierMargin="16dp"
            app:editPolygonMagnifier="#fff" />

Custom parameters `app:magnifierCrossSize`, `app:magnifierCrossStrokeWidth`, `app:magnifierStrokeWidth`, `app:magnifierRadius`, `app:magnifierMargin` and `app:editPolygonMagnifier` are optional.

Also, you should set up `MagnifierView` every time when editPolygonView is set with new image:

    magnifierView.setupMagnifier(editPolygonView);

And your `Activity` have to implement `DrawMagnifierListener`:

    public class MainActivity extends AppCompatActivity implements DrawMagnifierListener {
        
        @Override
        public void drawMagnifier(PointF zoomPoint) {
            magnifierView.drawMagnifier(zoomPoint);
        }
    
        @Override
        public void eraseMagnifier() {
            magnifierView.eraseMagnifier();
        }
    }

If you want to get selected polygon from `EditPolygonImageView` just call:

    editPolygonView.getPolygon();


  