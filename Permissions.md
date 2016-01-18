With SDK Package 1 you have a possibility to select which permissions to use in your app, depending on the set of SDK features which are used.

    android.permission.CAMERA

This permission is used for the camera views. 

Additionally, if you're using camera, you need to declare it as a used feature in your manifest:

    <uses-feature android:name="android.hardware.camera" />

---

    android.permission.WRITE_EXTERNAL_STORAGE
    android.permission.READ_EXTERNAL_STORAGE

Those permissions are used for PDF creation.