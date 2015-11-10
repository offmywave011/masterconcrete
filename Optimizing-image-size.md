To use `ImageQualityOptimizer` methods you need its instance first: ImageQualityOptimizer optimizer = new ImageQualityOptimizer(); 

You can choose from two variants of the same method:

Full version of `optimizeImage(Bitmap bitmap, float widthInches, float heightInches, long maxRequiredDpi)`. 
Where: 
* `bitmap` - source image you want to optimize
* `widthInches` - suppositious physical width of photo source page
* `heightInches` - suppositious physical height of photo source page
* `maxRequiredDpi` - desirable quality

Method returns optimized (scaled down) bitmap. If quality of source image will be less than `maxRequiredDpi`,then original bitmap  will be returned.

Shorter version of `optimizeImage(Bitmap bitmap, long requiredDpi)`.
You don't need to pass suppositious physical size of source page to it, but final result can be less precise.

Also contains `PageFormat` - common paper formats (width and height in inches).

Usage example:

`Bitmap optimizedImage = new ImageQualityOptimizer().optimizeImage(sourceImage, ImageQualityOptimizer.PaperFormat.A4.widthInches, ImageQualityOptimizer.PaperFormat.A4.heightInches, 100);`