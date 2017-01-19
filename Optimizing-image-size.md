To use the `ImageQualityOptimizer` methods you need their instance first: ImageQualityOptimizer optimizer = new ImageQualityOptimizer(); 

You can choose from two variants of the same method:

Full version of `optimizeImage(Bitmap bitmap, float widthInches, float heightInches, long maxRequiredDpi)`. 
Where: 
* `bitmap` - is the source image you want to optimize
* `widthInches` - is the suppositious physical width of photo source page
* `heightInches` - is the suppositious physical height of photo source page
* `maxRequiredDpi` - is the desirable quality

The method returns an optimized (scaled down) bitmap. If the quality of the source image will be less than `maxRequiredDpi`,then the original bitmap will be returned.

A shorter version of `optimizeImage(Bitmap bitmap, long requiredDpi)`.
You do not need to pass the suppositious physical size of a source page to it, but the final result might be less precise.

It also contains `PageFormat` - which refers to common paper formats (width and height in inches).

Usage example:

`Bitmap optimizedImage = new ImageQualityOptimizer().optimizeImage(sourceImage, ImageQualityOptimizer.PaperFormat.A4.widthInches, ImageQualityOptimizer.PaperFormat.A4.heightInches, 100);`