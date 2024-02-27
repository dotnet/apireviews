# API Review 02/27/2024

## Add support for converting the format of a Bitmap

**Approved** | [#winforms/8827](https://github.com/dotnet/winforms/issues/8827#issuecomment-1967454163) | [Video](https://www.youtube.com/watch?v=LIXgbP4Qxh0&t=0h0m0s)

```C#
namespace System.Drawing.Imaging
{
    public enum PaletteType
    {
        Custom,
        FixedBlackAndWhite,
        FixedHalftone8,
        FixedHalftone27,
        FixedHalftone64,
        FixedHalftone125,
        FixedHalftone216,
        FixedHalftone252,
        FixedHalftone256
    }
    
    public enum DitherType
    {
        None,
        Solid,
        ErrorDiffusion,
        Ordered4x4,
        Ordered8x8,
        Ordered16x16,
        Spiral4x4,
        Spiral8x8,
        DualSpiral4x4,
        DualSpiral8x8
    }

    public sealed class ColorPalette
    {
        public ColorPalette(params Color[] customColors);
        public ColorPalette(PaletteType fixedPaletteType);
        public static ColorPalette CreateOptimalPalette(int colors,
                                                        bool useTransparentColor,
                                                        Bitmap bitmap);
    }
}

namespace System.Drawing
{
    public sealed class Bitmap : System.Drawing.Image
    {
        public void ConvertFormat(PixelFormat format);
        public void ConvertFormat(PixelFormat format,
                                  DitherType ditherType,
                                  PaletteType paletteType = PaletteType.Custom,
                                  ColorPalette? palette = null,
                                  float alphaThresholdPercent = 0.0f);
    }
}
```
