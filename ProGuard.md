You have to add this ProGuard rules for all Scanbot SDK Packages:

```
-dontwarn
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*,!code/allocation/variable
-renamesourcefileattribute SourceFile
-keepattributes SourceFile,LineNumberTable,*Annotation*,Signature,InnerClasses

-keepclasseswithmembernames class * {
    native <methods>;
}

-keepclasseswithmembernames class * {
    public <init>(android.content.Context, android.util.AttributeSet);
}

-keepclasseswithmembernames class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
}

-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

-keep class * implements android.os.Parcelable {*;}

-keepattributes Exceptions

-keep interface android.support.v7.** { *; }
-keep class !android.support.v7.internal.view.menu.**, android.support.** {*;}

-keep public class * extends android.support.v7.app.ActionBarActivity { *; }
-keep class android.support.v7.widget.** { *; }

# Scanbot SDK
-keep public class net.doo.snap.ScanbotSDKInitializer {
    public <methods>;
}
-keep public class net.doo.snap.ScanbotSDK {
    public <methods>;
}
-keep public class net.doo.snap.Constants { *; }
-keep public class net.doo.snap.persistence.cleanup.Cleaner {
    public <methods>;
}
-keep public class net.doo.snap.intelligence.DocumentClassifier {
    public <methods>;
}
-keep public class net.doo.snap.intelligence.StubDocumentClassifier {
    public <methods>;
}
-keep public class net.doo.snap.persistence.cleanup.UnreferencedSourcesProvider {
    public <methods>;
}
-keep public class net.doo.snap.persistence.cleanup.BaseUnreferencedSourcesProvider {
    public <methods>;
}
-keep public class io.scanbot.sap.SapManager { *; }
-keep public class net.doo.snap.entity.Language { *; }

-keep public class net.doo.snap.PreferencesConstants  { *; }
-keep public class net.doo.snap.PreferencesConstants  { *; }
-keep public class net.doo.snap.util.log.DebugLog {
    public <methods>;
}
-keep public class net.doo.snap.persistence.DocumentStoreStrategy {
    public <methods>;
}
-keep public class net.doo.snap.entity.DocumentType {
    public <methods>;
}
-keep public class net.doo.snap.entity.OcrStatus { *; }
-keep public class net.doo.snap.persistence.PageStoreStrategy {
    public <methods>;
}
-keep public class net.doo.snap.entity.Page { *; }
-keep enum net.doo.snap.entity.Page$* { *; }
-keep public class net.doo.snap.entity.RotationType { *; }
-keep public class net.doo.snap.IntentExtras { *; }
-keep public class net.doo.snap.connectivity.ConnectionChecker {
    public <methods>;
}
-keep public class net.doo.snap.BuildConfig { *; }
-keep public class net.doo.snap.entity.DocumentType { *; }
-keep public class net.doo.snap.persistence.PageFactory {
    public <methods>;
}
-keep public class net.doo.snap.blob.BlobManager { *; }
-keep public class net.doo.snap.blob.BlobFactory { *; }
-keep public class net.doo.snap.entity.Blob { *; }
-keep public class net.doo.snap.persistence.BlobStoreStrategy { *; }

-keep public class net.doo.snap.persistence.PageFactory$Result { *; }

-keep public class net.doo.snap.camera.** { *; }

-keep public class net.doo.snap.ui.** { *; }
-keep public class net.doo.snap.util.** { *; }
-keep public class net.doo.snap.process.** { *; }

-keepclassmembers class * {
    void *(**Draft);
}

-keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    public void get*(...);
}

# Roboguice

-keep class com.google.inject.Binder

-keepclassmembers class * {
    @com.google.inject.Inject <init>(...);
    @com.google.inject.Inject <fields>;
    @javax.annotation.Nullable <fields>;
}

-keep class com.google.inject.** { *; }
-keep class javax.inject.** { *; }
-keep class javax.annotation.** { *; }

-keep class roboguice.** { *; }
-dontwarn roboguice.**
-dontwarn org.roboguice.**
-dontwarn com.google.inject.**
-dontwarn com.google.common.**
-dontwarn org.apache.log4j.**

# App specific

-keep public class * extends com.google.inject.AbstractModule {*;}

-keeppackagenames net.doo.snap.lib.detector.**
-keep public class net.doo.snap.lib.detector.**{ *; }

-keep class * extends java.util.ListResourceBundle {
    protected Object[][] getContents();
}

-keepnames class * implements android.os.Parcelable {
    public static final ** CREATOR;
}

-keep public class com.google.android.gms.common.internal.safeparcel.SafeParcelable {
    public static final *** NULL;
}

-keepnames @com.google.android.gms.common.annotation.KeepName class *
-keepclassmembernames class * {
    @com.google.android.gms.common.annotation.KeepName *;
}

-keeppackagenames com.googlecode.tesseract.android.**
-keep public class com.googlecode.tesseract.android.**{ *; }
```