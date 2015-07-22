For document uploading follow next steps: 

1. Upload to Cloud Storages is provided in the full Scanbot SDK package. You have to add 

    `compile "io.scanbot:sdk-package-full:$latestVersion"`

    to your Gradle dependencies.

2. For Dropbox, Onedrive, Amazon Cloud storage, Slack, Wunderlist, Evernote, Yandex and Shoeboxed services you have to add client id, client secret and redirect url (or only some of them) as meta-data to AndroidManifest.xml.

    E.g. for Dropbox:

    `<meta-data android:name="dropbox_app_key" android:value="INSERT_APP_KEY" />`

    `<meta-data android:name="dropbox_app_secret" android:value="INSERT_APP_SECRET" />`

    All required meta-data for cloud storages you can find in our cloud-upload example application.

    Also, auth activites for Dropbox and Yandex Disk storages have to be added to AndroidManifest.xml.

        <!-- This activity is needed only for Dropbox SDK authorization.
        https://www.dropbox.com/developers/core/sdks/android-->
        <activity
            android:name="com.dropbox.client2.android.AuthActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:launchMode="singleTask">
        
            <intent-filter>
                <!-- Change this to be db- followed by your app key -->
                <data android:scheme="db-INSERT_APP_KEY" />
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.BROWSABLE" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>  
        </activity>

        <!-- This activity is needed only for Yandex Disk SDK authorization.-->
        <activity
            android:name="net.doo.snap.ui.upload.AuthYandexDiskActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:launchMode="singleTask"
            android:theme="@android:style/Theme.Holo.Light.NoActionBar">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.BROWSABLE" />
                <category android:name="android.intent.category.DEFAULT" />

                <data
                    android:host="INSERT_HOST_HERE"
                    android:scheme="INSERT_SCHEME_HERE" />
            </intent-filter>
        </activity>

    Replace the host and scheme with your values for Dropbox and Yandex Disk activities.

3. Add `DocumentUploader` inject:

        @Inject
        private DocumentUploader documentUploader;
 
    You can find all supported cloud storages in `CloudStorage` enum.
    Before file uploading, you have to authorize user in the target cloud storage. To do this you have to check if a cloud storage is connected:
    
   `CloudStorage.isConnected(selectedStorage, MainActivity.this)`

    If storage is not connected you have to start for result the authorization activity:

        startActivityForResult(
            new Intent(
                    MainActivity.this, 
                    selectedStorage.getAuthActivityClass()), 
            IntentExtras.AUTH_REQUEST_CODE);
    
    In `onActivityResult(int requestCode, int resultCode, Intent data)` method you have to check authorization result:

        if (requestCode == IntentExtras.AUTH_REQUEST_CODE && resultCode == Activity.RESULT_OK) {
            ConnectionResult event = data.getParcelableExtra(IntentExtras.CONNECTION_EXTRA);
            if (event.isConnected()) {
                //when cloud storage successfully connected - start file uploading
             } else {
                Toast.makeText(MainActivity.this, "Cannot connect to cloud storage!", Toast.LENGTH_LONG).show();
             }
         }    

4. When user will be successfully authorized in the cloud storage, you can start file upload:

    `documentUploader.uploadDocument(uploadInfo, selectedStorage, new OnFileUploadListenerExample());`

    `uploadDocument()` method takes `UploadInfo` object as one of parameters. This object contains all information about the file and the target cloud storage. In `UploadInfo` constuctor only `File` parameter is required for successful upload. All other parameters are optional and can be set as `null`. 

    Exception is only for Slack storage. It requires also extra information about the target channel, direct message or group, this data represented as `Bandle` parameter in `UploadInfo` constuctor. To getting this `Bundle` before `uploadDocument()` call, you have to show `SlackManualUploadFragment`:

        SlackManualUploadFragment dialog = (SlackManualUploadFragment) CloudStorage.getManualUploadFragment(target, uploadInfos);
        dialog.show(fragmentManager, TAG);

    For this dialog you can be set `SlackManualUploadFragment.SlackUploadListener` which reacts on the group, direct message or channel selection, and takes `Bundle` with selected information.      
    
5. `uploadDocument()` method also takes `OnFileUploadListener` as a parameter. This listener can be used for handling upload results. There are 3 types of results it can handle: 
    * upload finished successfully;
    * upload failed - e.g. internet connection problems;
    * upload authorization failed - in this case most likely auth tokens are invalid and you have to disconnect current account `CloudStorage.disconnect(storage, context);` and start authorization again.