# API Review 02/20/2024

## Support rounded rectangle in Graphics class

**Approved** | [#winforms/9001](https://github.com/dotnet/winforms/issues/9001#issuecomment-1954801001)

* Based on the naming from GDI and from WPF, `Size corner` => `Size radius`.

```diff
namespace System.Drawing;

public class Graphics
{
     public void DrawRectangle(System.Drawing.Pen pen, System.DrawingRectangle rect);
     // ...
+    public void DrawRoundedRectangle(System.Drawing.Pen pen, System.Drawing.Rectangle rect, Size radius);
+    public void DrawRoundedRectangle(System.Drawing.Pen pen, System.Drawing.RectangleF rect, SizeF radius);
+    public void FillRoundedRectangle(System.Drawing.Brush brush, System.Drawing.Rectangle rect, Size radius);
+    public void FillRoundedRectangle(System.Drawing.Brush brush, System.Drawing.RectangleF rect, SizeF radius);
}

namespace System.Drawing.Drawing2D;

public class GraphicsPath
{
     public void AddRectangle(Rectangle rect);
     // ...
+    public void AddRoundedRectangle(Rectangle rect, Size radius);
+    public void AddRoundedRectangle(RectangleF rect, SizeF radius);
}
```
## Add Span overloads to System.Drawing APIs

**Approved** | [#winforms/10763](https://github.com/dotnet/winforms/issues/10763#issuecomment-1954883178)

* GraphicsPath.TryGetPathTypes/TryGetPathPoints changed to non-Try variants and changed the span name to `destination`.
* `ValueColorMap` is replaced with `(Color OldColor, Color NewColor)`.
* Adjusted ImageAttributes to also do params-array for SetBrushRemapTable/SetRemapTable.
* The existing SetRemapTable is now marked as EB-Never so .NET9+ callers won't see the shape twist in overloads.

```diff
namespace System.Drawing;

public class Graphics
{
-   public void DrawRectangles(Pen pen, Rectangle[] rects);
+   public void DrawRectangles(Pen pen, params Rectangle[] rects);
+   public void DrawRectangles(Pen pen, params ReadOnlySpan<Rectangle> rects);
-   public void DrawRectangles(Pen pen, RectangleF[] rects);
+   public void DrawRectangles(Pen pen, params RectangleF[] rects);
+   public void DrawRectangles(Pen pen, params ReadOnlySpan<RectangleF> rects);
-   public void DrawPolygon(Pen pen, Point[] points);
+   public void DrawPolygon(Pen pen, params Point[] points);
+   public void DrawPolygon(Pen pen, params ReadOnlySpan<Point> points);
-   public void DrawPolygon(Pen pen, PointF[] points);
+   public void DrawPolygon(Pen pen, params PointF[] points);
+   public void DrawPolygon(Pen pen, params ReadOnlySpan<PointF> points);
-   public void DrawCurve(Pen pen, Point[] points);
+   public void DrawCurve(Pen pen, params Point[] points);
+   public void DrawCurve(Pen pen, params ReadOnlySpan<Point> points);
    public void DrawCurve(Pen pen, Point[] points, float tension);
+   public void DrawCurve(Pen pen, ReadOnlySpan<Point> points, float tension);
    public void DrawCurve(Pen pen, Point[] points, int offset, int numberOfSegments);
+   public void DrawCurve(Pen pen, ReadOnlySpan<Point> points, int offset, int numberOfSegments);
    public void DrawCurve(Pen pen, Point[] points, int offset, int numberOfSegments, float tension);
+   public void DrawCurve(Pen pen, ReadOnlySpan<Point> points, int offset, int numberOfSegments, float tension);
-   public void DrawCurve(Pen pen, PointF[] points);
+   public void DrawCurve(Pen pen, params PointF[] points);
+   public void DrawCurve(Pen pen, params ReadOnlySpan<PointF> points);
    public void DrawCurve(Pen pen, PointF[] points, float tension);
+   public void DrawCurve(Pen pen, ReadOnlySpan<PointF> points, float tension);
    public void DrawCurve(Pen pen, PointF[] points, int offset, int numberOfSegments);
+   public void DrawCurve(Pen pen, ReadOnlySpan<PointF> points, int offset, int numberOfSegments);
    public void DrawCurve(Pen pen, PointF[] points, int offset, int numberOfSegments, float tension);
+   public void DrawCurve(Pen pen, ReadOnlySpan<PointF> points, int offset, int numberOfSegments, float tension);
-   public void DrawClosedCurve(Pen pen, Point[] points);
+   public void DrawClosedCurve(Pen pen, params Point[] points);
+   public void DrawClosedCurve(Pen pen, params ReadOnlySpan<Point> points);
    public void DrawClosedCurve(Pen pen, Point[] points, float tension, FillMode fillmode);
+   public void DrawClosedCurve(Pen pen, ReadOnlySpan<Point> points, float tension, FillMode fillmode);
-   public void DrawClosedCurve(Pen pen, PointF[] points);
+   public void DrawClosedCurve(Pen pen, params PointF[] points);
+   public void DrawClosedCurve(Pen pen, params ReadOnlySpan<PointF> points);
    public void DrawClosedCurve(Pen pen, PointF[] points, float tension, FillMode fillmode);
+   public void DrawClosedCurve(Pen pen, ReadOnlySpan<PointF> points, float tension, FillMode fillmode);
-   public void FillRectangles(Brush brush, Rectangle[] rects);
+   public void FillRectangles(Brush brush, params Rectangle[] rects);
+   public void FillRectangles(Brush brush, params ReadOnlySpan<Rectangle> rects);
-   public void FillPolygon(Brush brush, Point[] points);
+   public void FillPolygon(Brush brush, params Point[] points);
+   public void FillPolygon(Brush brush, params ReadOnlySpan<Point> points);
    public void FillPolygon(Brush brush, Point[] points, FillMode fillMode);
+   public void FillPolygon(Brush brush, ReadOnlySpan<Point> points, FillMode fillMode)
-   public void FillRectangles(Brush brush, RectangleF[] rects)
+   public void FillRectangles(Brush brush, params RectangleF[] rects)
+   public void FillRectangles(Brush brush, params ReadOnlySpan<RectangleF> rects);
-   public void FillPolygon(Brush brush, PointF[] points);
+   public void FillPolygon(Brush brush, params PointF[] points);
+   public void FillPolygon(Brush brush, params ReadOnlySpan<PointF> points);
    public void FillPolygon(Brush brush, PointF[] points, FillMode fillMode);
+   public void FillPolygon(Brush brush, ReadOnlySpan<PointF> points, FillMode fillMode);
-   public void FillClosedCurve(Brush brush, Point[] points);
+   public void FillClosedCurve(Brush brush, params Point[] points);
+   public void FillClosedCurve(Brush brush, params ReadOnlySpan<Point> points);
    public void FillClosedCurve(Brush brush, Point[] points, FillMode fillmode);
+   public void FillClosedCurve(Brush brush, ReadOnlySpan<Point> points, FillMode fillmode);
-   public void FillClosedCurve(Brush brush, PointF[] points);
+   public void FillClosedCurve(Brush brush, params PointF[] points);
+   public void FillClosedCurve(Brush brush, params ReadOnlySpan<PointF> points);
    public void FillClosedCurve(Brush brush, PointF[] points, FillMode fillmode);
+   public void FillClosedCurve(Brush brush, ReadOnlySpan<PointF> points, FillMode fillmode);
-   public void DrawLines(Pen pen, Point[] points);
+   public void DrawLines(Pen pen, params Point[] points);
+   public void DrawLines(Pen pen, params ReadOnlySpan<Point> points);
-   public void DrawLines(Pen pen, PointF[] points);
+   public void DrawLines(Pen pen, params PointF[] points);
+   public void DrawLines(Pen pen, params ReadOnlySpan<PointF> points);
-   public void DrawBeziers(Pen pen, Point[] points);
+   public void DrawBeziers(Pen pen, params Point[] points);
+   public void DrawBeziers(Pen pen, params ReadOnlySpan<Point> points);
-   public void DrawBeziers(Pen pen, PointF[] points);
+   public void DrawBeziers(Pen pen, params PointF[] points);
+   public void DrawBeziers(Pen pen, params ReadOnlySpan<PointF> points);
}

namespace System.Drawing.Drawing2D;

public class GraphicsPath
{
    public GraphicsPath(Point[] pts, byte[] types);
    public GraphicsPath(Point[] pts, byte[] types, FillMode fillMode);
+   public GraphicsPath(ReadOnlySpan<Point> pts, ReadOnlySpan<byte> types, FillMode fillMode = FillMode.Alternate);
    public GraphicsPath(PointF[] pts, byte[] types);
    public GraphicsPath(PointF[] pts, byte[] types, FillMode fillMode);
+   public GraphicsPath(ReadOnlySpan<PointF> pts, ReadOnlySpan<byte> types, FillMode fillMode = FillMode.Alternate);
-   public void AddLines(Point[] points);
+   public void AddLines(params Point[] points);
+   public void AddLines(params ReadOnlySpan<Point> points);
-   public void AddLines(PointF[] points);
+   public void AddLines(params PointF[] points);
+   public void AddLines(params ReadOnlySpan<PointF> points);
    public void AddBeziers(params Point[] points);
+   public void AddBeziers(params ReadOnlySpan<Point> points);
-   public void AddBeziers(PointF[] points);
+   public void AddBeziers(params PointF[] points);
+   public void AddBeziers(params ReadOnlySpan<PointF> points);
-   public void AddCurve(Point[] points);
+   public void AddCurve(params Point[] points);
+   public void AddCurve(params ReadOnlySpan<Point> points);
    public void AddCurve(Point[] points, float tension);
+   public void AddCurve(ReadOnlySpan<Point> points, float tension);
    public void AddCurve(Point[] points, int offset, int numberOfSegments, float tension);
+   public void AddCurve(ReadOnlySpan<Point> points, int offset, int numberOfSegments, float tension);
-   public void AddCurve(PointF[] points);
+   public void AddCurve(params PointF[] points);
+   public void AddCurve(params ReadOnlySpan<PointF> points);
    public void AddCurve(PointF[] points, float tension);
+   public void AddCurve(ReadOnlySpan<PointF> points, float tension);
    public void AddCurve(PointF[] points, int offset, int numberOfSegments, float tension);
+   public void AddCurve(ReadOnlySpan<PointF> points, int offset, int numberOfSegments, float tension);
-   public void AddClosedCurve(Point[] points);
+   public void AddClosedCurve(params Point[] points);
+   public void AddClosedCurve(params ReadOnlySpan<Point> points);
    public void AddClosedCurve(Point[] points, float tension);
+   public void AddClosedCurve(ReadOnlySpan<Point> points, float tension);
-   public void AddRectangles(Rectangle[] rects);
+   public void AddRectangles(params Rectangle[] rects);
+   public void AddRectangles(params ReadOnlySpan<Rectangle> rects);
-   public void AddPolygon(PointF[] points);
+   public void AddPolygon(params PointF[] points);
+   public void AddPolygon(params ReadOnlySpan<PointF; points);
-   public void AddClosedCurve(PointF[] points);
+   public void AddClosedCurve(params PointF[] points);
+   public void AddClosedCurve(params ReadOnlySpan<PointF> points);
    public void AddClosedCurve(PointF[] points, float tension);
+   public void AddClosedCurve(ReadOnlySpan<PointF> points, float tension);
-   public void AddRectangles(RectangleF[] rects);
+   public void AddRectangles(params RectangleF[] rects);
+   public void AddRectangles(params ReadOnlySpan<RectangleF> rects);
-   public void AddPolygon(PointF[] points);
+   public void AddPolygon(params PointF[] points);
+   public void AddPolygon(params ReadOnlySpan<PointF; points);
    public void Warp(PointF[] destPoints, RectangleF srcRect, Matrix? matrix, WarpMode warpMode, float flatness);
+   public void Warp(ReadOnlySpan<PointF> destPoints, RectangleF srcRect, Matrix? matrix = default, WarpMode warpMode WarpMode.Perspective, float flatness = 0.25f);
    public byte[] PathTypes { get; }
+   public int GetPathTypes(Span<bytes> destination);
    
    // Everything in GDI+ is float internally, so there is no Point version of this
    public PointF[] PathPoints { get; }
+   public int GetPathPoints(Span<PointF> destination);
}

public class ImageAttributes
{
-   public void SetBrushRemapTable(ColorMap[] map);
+   public void SetBrushRemapTable(params ColorMap[] map);
+   public void SetBrushRemapTable(params ReadOnlySpan<ColorMap> map);
+   public void SetBrushRemapTable(params ReadOnlySpan<(Color OldColor, Color NewColor)> map);

-   public void SetRemapTable(ColorMap[] map);
+   public void SetRemapTable(params ColorMap[] map);
+   public void SetRemapTable(params ReadOnlySpan<ColorMap> map);
+   public void SetRemapTable(params ReadOnlySpan<(Color OldColor, Color NewColor)> map);
+   public void SetRemapTable(ColorAdjustType type, params ColorMap[] map);
+   public void SetRemapTable(ColorAdjustType type, params ReadOnlySpan<ColorMap> map);
+   public void SetRemapTable(ColorAdjustType type, params ReadOnlySpan<(Color OldColor, Color NewColor)> map);

+   [EditorBrowsable(EditorBrowsableState.Never)]
    public void SetRemapTable(ColorMap[] map, ColorAdjustType type);
}
```
## Add generic interfaces to PrinterSettings.StringCollection

**Approved** | [#winforms/10795](https://github.com/dotnet/winforms/issues/10795#issuecomment-1954905004)

* We believe `IEnumerable<string>` is adequate for this scenario, and `IReadOnlyList<string>` is unnecessary.

```diff
namespace System.Drawing.Printing;

-public class StringCollection : ICollection
+public class StringCollection : ICollection, IEnumerable<string>
{
+    IEnumerator<string> IEnumerable<string>.GetEnumerator();
}
```

## Add Effects to System.Drawing

**Approved** | [#winforms/8835](https://github.com/dotnet/winforms/issues/8835#issuecomment-1954987873)

* ColorLookupTable's properties should be `ReadOnlyMemory<byte>`
* Effect as proposed is problematic because it is all of
  * It has a finalizer
  * The type hierarchy is not closed off (it can be extended outside the assembly)
  * It isn't following the Basic Dispose Pattern.
  * It sounds like the conclusion is to change to following the Basic Dispose Pattern.
* CurveChannel's enum members should have their CurveChannel prefix removed.
* The types derived from ColorMatrixEffect should be sealed.
* All of the ColorCurveEffect-derived types should use the suffix "CurveEffect"
* RedEyeCorrectionEffect, added `params` because of how many `params` were added in the previous issue.
* RedEyeCorrectionEffect, added `public ReadOnlyMemory<Point> Areas { get; }` since all the other types expose their ctor parameters as properties.
* System.Drawing.Graphics, added the simplified overload of DrawImage with no defaulted parameters.

```c#
namespace System.Drawing.Imaging.Effects;

public abstract class Effect : IDisposable
{
    // Cannot be created outside of System.Drawing assembly - has private protected constructor
    private protected Effect();
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    ~Effect();
}

// All derived classes don't actually exist as classes in GDI+, but as the int adjustment value bounds are
// so different for each effect type it seems better from a documentation perspective that subclasses with
// fully documented <param>'s are better than having to cross lookup against an enum of ColorCurveEffect types.
public abstract class ColorCurveEffect : Effect
{
    // Cannot be created outside of System.Drawing assembly - has private protected constructor
    private protected Effect();
    public CurveChannel Channel { get; }
}

public enum CurveChannel
{
    // Directly mapped to GDI+ values
    All,
    Red,
    Green,
    Blue,
}

// Basically a black point per channel effect
public class BlackSaturationCurveEffect : ColorCurveEffect
{
    public BlackSaturationCurveEffect(CurveChannel channel, int blackSaturation);
    public int BlackSaturation { get; }
}

public class BlurEffect : Effect
{
    public BlurEffect(float radius, bool expandEdge) ;
    public float Radius { get; }
    public bool ExpandEdge { get; }
}

public class BrightnessContrastEffect : Effect
{
    public BrightnessContrastEffect(int brightnessLevel, int contrastLevel);
    public int BrightnessLevel { get; }
    public int ContrastLevel { get; }
}

public class ColorBalanceEffect : Effect
{
    public ColorBalanceEffect(int cyanRed, int magentaGreen, int yellowBlue) : base(PInvoke.ColorBalanceEffectGuid);
    public int CyanRed { get; }
    public int MagentaGreen { get; }
    public int YellowBlue { get; }
}

public class ColorLookupTableEffect : Effect
{
    // A copy of lookup table data is kept in the class
    public ColorLookupTableEffect(
        ReadOnlySpan<byte> redLookupTable,
        ReadOnlySpan<byte> greenLookupTable,
        ReadOnlySpan<byte> blueLookupTable,
        ReadOnlySpan<byte> alphaLookupTable);

    public ColorLookupTableEffect(
        byte[] redLookupTable,
        byte[] greenLookupTable,
        byte[] blueLookupTable,
        byte[] alphaLookupTable);

    public ReadOnlyMemory<byte> RedLookupTable { get; }
    public ReadOnlyMemory<byte> GreenLookupTable { get; }
    public ReadOnlyMemory<byte> BlueLookupTable { get; }
    public ReadOnlyMemory<byte> AlphaLookupTable { get; }
}

public class ColorMatrixEffect : Effect
{
    public ColorMatrixEffect(ColorMatrix matrix);

    // ColorMatrix is a mutable class, but won't change what this effect does.
    public ColorMatrix Matrix { get; }
}

// These are just some predefined ColorMatrix values to cover some common scenarios.

public sealed class GrayScaleEffect : ColorMatrixEffect
{
    public GrayScaleEffect();
}

public sealed class SepiaEffect : ColorMatrixEffect
{
    public SepiaEffect();
}

public sealed class VividEffect : ColorMatrixEffect
{
    public VividEffect();
}

public sealed class InvertEffect : ColorMatrixEffect
{
    public InvertEffect();
}

// Different from BrightnessContrast in that you can adjust a single channel (and the implementation seems completely different)
public class ContrastCurveEffect : ColorCurveEffect
{
    public ContrastCurveEffect(CurveChannel channel, int contrast);
    public int Contrast { get; }
}

public class DensityCurveEffect : ColorCurveEffect
{
    public DensityCurveEffect(CurveChannel channel, int density);
    public int Density { get; }
}

public class ExposureCurveEffect : ColorCurveEffect
{
    public ExposureCurveEffect(CurveChannel channel, int exposure);
    public int Exposure { get; }
}

public class HighlightCurveEffect : ColorCurveEffect
{
    public HighlightCurveEffect(CurveChannel channel, int highlight);
    public int Highlight { get; }
}

public class LevelsEffect : Effect
{
    public LevelsEffect(int highlight, int midtone, int shadow);
    public int Highlight { get; }
    public int Midtone { get; }
    public int Shadow { get; }
}

public class MidtoneCurveEffect : ColorCurveEffect
{
    public MidtoneCurveEffect(CurveChannel channel, int midtone);
    public int Midtone { get; }
}

public class RedEyeCorrectionEffect: Effect
{
    public RedEyeCorrectionEffect(params ReadOnlySpan<Point> areas);
    public RedEyeCorrectionEffect(params Point[] areas);
    // As this is a one and done sort of effect I don't want to expose the areas to avoid unnecessary copies.
    // Or could and say that this will allocate? All GDI+ effects make a copy the incoming data.
    // public Point[] Areas { get; }

    public ReadOnlyMemory<Point> Areas { get; }
}

public class ShadowCurveEffect : ColorCurveEffect
{
    public ShadowEffect(CurveChannel channel, int shadow);
    public int Shadow { get; }
}

public class SharpenEffect : Effect
{
    public SharpenEffect(float radius, float amount);
    public float Radius => _sharpenParams.radius;
    public float Amount => _sharpenParams.amount;
}

public class TintEffect : Effect
{
    // Convenience constructor. It's lossy so I don't expose Color in a getter.
    public TintEffect(Color color, int amount);
    // This is the only effect where I've changed behavior. The hue goes from -180 to 180 in GDI+ on this API,
    // but Color.GetHue() goes from 0 to 360. As such, this takes 0-360.
    public TintEffect(int hue, int amount);
    public int Hue { get; }
    public int Amount { get; }
}

public class WhiteSaturationCurveEffect : ColorCurveEffect
{
    public WhiteSaturationEffect(CurveChannel channel, int whiteSaturation);
    public int WhiteSaturation { get; }
}

namespace System.Drawing;

public partial class Bitmap
{
    // Empty rectangle is the entire image
    public void ApplyEffect(Effect effect, Rectangle area = default);
}

public partial class Graphics
{
    public void DrawImage(Image image, Effect effect);

    public void DrawImage(
        Image image,
        Effect effect,
        RectangleF srcRect = default,
        Matrix? transform = default,
        GraphicsUnit srcUnit = GraphicsUnit.Pixel,
        ImageAttributes? imageAttr = default);
}

namespace System.Drawing.Imaging;

public sealed partial class ColorMatrix
{
    public ColorMatrix(ReadOnlySpan<float> newColorMatrix);
}
```
