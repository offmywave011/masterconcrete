You have to add this ProGuard rules for all Scanbot SDK Packages:

```
-ignorewarnings

-keep public class net.doo.snap.ui.** { *; }

-keep public class io.scanbot.sap.SapManager { *; }

-keeppackagenames net.doo.snap.lib.detector.**
-keep public class net.doo.snap.lib.detector.**{ *; }

-keeppackagenames com.googlecode.tesseract.android.**
-keep public class com.googlecode.tesseract.android.**{ *; }

-keeppackagenames io.scanbot.payformscanner.**
-keep public class io.scanbot.payformscanner.**{ *; }

-keeppackagenames io.scanbot.mrzscanner.**
-keep public class io.scanbot.mrzscanner.**{ *; }

-keeppackagenames io.scanbot.dcscanner.**
-keep public class io.scanbot.dcscanner.**{ *; }

-keeppackagenames io.scanbot.tiffwriter.**
-keep public class io.scanbot.tiffwriter.**{ *; }

-keeppackagenames io.scanbot.chequescanner.**
-keep public class io.scanbot.chequescanner.**{ *; }

-keeppackagenames io.scanbot.textorientation.**
-keep public class io.scanbot.textorientation.**{ *; }

-keeppackagenames io.scanbot.barcodescanner.**
-keep public class io.scanbot.barcodescanner.**{ *; }

-keeppackagenames io.scanbot.hicscanner.**
-keep public class io.scanbot.hicscanner.**{ *; }
```