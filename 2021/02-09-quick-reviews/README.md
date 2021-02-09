# Quick Reviews 02/09/2021

## More performant overloads for Graphics.GetContextInfo

**Approved** | [#runtime/47880](https://github.com/dotnet/runtime/issues/47880#issuecomment-776141547) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h0m0s)

* The existing `GetContextInfo()` is hidden from IntelliSense. We should mark it as `[Obsolete]` and point people to the new APIs, at the off chance that someone is using the old one.
* Otherwise, looks good.

```C#
namespace System.Drawing
{
    public sealed class Graphics
    {
        [Obsolete("Use one of the other overloads.")]
        public object GetContextInfo();
        public void GetContextInfo(out PointF offset);
        public void GetContextInfo(out PointF offset, out Region clip);
    }
}
```

## Add IDeviceContext overloads to ControlPaint

**Approved** | [#winforms/4338](https://github.com/dotnet/winforms/issues/4338#issuecomment-776147268) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h16m26s)

* Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public partial class ControlPaint
    {
        public static void DrawBorder(IDeviceContext deviceContext, Rectangle bounds, Color color, ButtonBorderStyle style);
        public static void DrawBorder(IDeviceContext deviceContext, Rectangle bounds, Color leftColor, int leftWidth, ButtonBorderStyle leftStyle, Color topColor, int topWidth, ButtonBorderStyle topStyle, Color rightColor, int rightWidth, ButtonBorderStyle rightStyle, Color bottomColor, int bottomWidth, ButtonBorderStyle bottomStyle);
        public static void DrawBorder3D(IDeviceContext deviceContext, int x, int y, int width, int height);
        public static void DrawBorder3D(IDeviceContext deviceContext, int x, int y, int width, int height, Border3DStyle style);
        public static void DrawBorder3D(IDeviceContext deviceContext, int x, int y, int width, int height, Border3DStyle style, Border3DSide sides);
        public static void DrawBorder3D(IDeviceContext deviceContext, Rectangle rectangle);
        public static void DrawBorder3D(IDeviceContext deviceContext, Rectangle rectangle, Border3DStyle style);
        public static void DrawBorder3D(IDeviceContext deviceContext, Rectangle rectangle, Border3DStyle style, Border3DSide sides);
        public static void DrawButton(IDeviceContext deviceContext, int x, int y, int width, int height, ButtonState state);
        public static void DrawButton(IDeviceContext deviceContext, Rectangle rectangle, ButtonState state);
        public static void DrawCaptionButton(IDeviceContext deviceContext, int x, int y, int width, int height, CaptionButton button, ButtonState state);
        public static void DrawCaptionButton(IDeviceContext deviceContext, Rectangle rectangle, CaptionButton button, ButtonState state);
        public static void DrawCheckBox(IDeviceContext deviceContext, int x, int y, int width, int height, ButtonState state);
        public static void DrawCheckBox(IDeviceContext deviceContext, Rectangle rectangle, ButtonState state);
        public static void DrawComboButton(IDeviceContext deviceContext, int x, int y, int width, int height, ButtonState state);
        public static void DrawComboButton(IDeviceContext deviceContext, Rectangle rectangle, ButtonState state);
        public static void DrawContainerGrabHandle(IDeviceContext deviceContext, Rectangle bounds);
        public static void DrawFocusRectangle(IDeviceContext deviceContext, Rectangle rectangle);
        public static void DrawFocusRectangle(IDeviceContext deviceContext, Rectangle rectangle, Color foreColor, Color backColor);
        public static void DrawGrabHandle(IDeviceContext deviceContext, Rectangle rectangle, bool primary, bool enabled);
        public static void DrawGrid(IDeviceContext deviceContext, Rectangle area, Size pixelsBetweenDots, Color backColor);
        public static void DrawLockedFrame(IDeviceContext deviceContext, Rectangle rectangle, bool primary);
        public static void DrawMenuGlyph(IDeviceContext deviceContext, int x, int y, int width, int height, MenuGlyph glyph);
        public static void DrawMenuGlyph(IDeviceContext deviceContext, int x, int y, int width, int height, MenuGlyph glyph, Color foreColor, Color backColor);
        public static void DrawMenuGlyph(IDeviceContext deviceContext, Rectangle rectangle, MenuGlyph glyph);
        public static void DrawMenuGlyph(IDeviceContext deviceContext, Rectangle rectangle, MenuGlyph glyph, Color foreColor, Color backColor);
        public static void DrawMixedCheckBox(IDeviceContext deviceContext, int x, int y, int width, int height, ButtonState state);
        public static void DrawMixedCheckBox(IDeviceContext deviceContext, Rectangle rectangle, ButtonState state);
        public static void DrawRadioButton(IDeviceContext deviceContext, int x, int y, int width, int height, ButtonState state);
        public static void DrawRadioButton(IDeviceContext deviceContext, Rectangle rectangle, ButtonState state);
        public static void DrawScrollButton(IDeviceContext deviceContext, int x, int y, int width, int height, ScrollButton button, ButtonState state);
        public static void DrawScrollButton(IDeviceContext deviceContext, Rectangle rectangle, ScrollButton button, ButtonState state);
        public static void DrawSelectionFrame(IDeviceContext deviceContext, bool active, Rectangle outsideRect, Rectangle insideRect, Color backColor);
        public static void DrawSizeGrip(IDeviceContext deviceContext, Color backColor, int x, int y, int width, int height);
        public static void DrawSizeGrip(IDeviceContext deviceContext, Color backColor, Rectangle bounds);
        public static void DrawVisualStyleBorder(IDeviceContext deviceContext, Rectangle bounds);

        // Omitted, IDeviceContext doesn't make sense as this also takes Image
        // public static void DrawImageDisabled(Graphics graphics, Image image, int x, int y, Color background);

        // Already exists
        // public static void DrawStringDisabled(IDeviceContext deviceContext, string s, Font font, Color color, RectangleF layoutRectangle, StringFormat format);
    }
}
```

## Expose `System.Drawing.Graphics.NativeGraphics`.

**Approved** | [#runtime/39406](https://github.com/dotnet/runtime/issues/39406#issuecomment-776150895) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h25m11s)

* Looks good as proposed

```C#
namespace System.Drawing
{
    public partial class Graphics
    {
        public IntPtr Handle { get; }
        public static Graphics FromHandle(IntPtr handle);
    }
    public partial class Brush
    {
        public IntPtr Handle { get; }
        public static Brush FromHandle(IntPtr handle);
    }
    public partial class Pen
    {
        public IntPtr Handle { get; }
        public static Pen FromHandle(IntPtr handle);
    }
    public partial class Image
    {
        public IntPtr Handle { get; }
        public static Image FromHandle(IntPtr handle);
    }

    public partial class Region
    {
        public IntPtr Handle { get; }
        public static Region FromHandle(IntPtr handle);
    }
}
namespace System.Drawing.Drawing2D
{
    public partial class Matrix
    {
        public IntPtr Handle { get; }
        public static Matrix FromHandle(IntPtr handle);
    }

    public partial class GraphicsPath
    {
        public IntPtr Handle { get; }
        public static GraphicsPath FromHandle(IntPtr handle);
    }
}
```

## Add more performant transform APIs in System.Drawing

**Approved** | [#runtime/47940](https://github.com/dotnet/runtime/issues/47940#issuecomment-776158368) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h31m7s)

* We should add constructors and an instance method to perform the conversions

```C#
namespace System.Drawing
{
    public partial class Graphics
    {
        // Exists:
        // public Matrix Transform { get; set; }
        public Matrix3x2 TransformElements { get; set; }
    }
    public partial struct PointF
    {
        public PointF(Vector2 vector);
        public Vector2 ToVector2();
        public static explicit operator Vector2(PointF point);
        public static explicit operator PointF(Vector2 vector);
    }
    public partial struct SizeF
    {
        public SizeF(Vector2 vector);
        public Vector2 ToVector2();
        public static explicit operator Vector2(SizeF size);
        public static explicit operator SizeF(Vector2 vector);
    }
    public partial struct RectangleF
    {
        public RectangleF(Vector4 vector);
        public Vector4 ToVector4();
        public static explicit operator Vector4(RectangleF rectangle);
        public static explicit operator RectangleF(Vector4 vector);
    }
}
namespace System.Drawing.Drawing2D
{
    public partial class Matrix
    {
        public Matrix(Matrix3x2 matrix);
        // Exists:
        // public float[] Elements { get; }
        public Matrix3x2 MatrixElements { get; set; }
    }
}
```

## Add an Enumerable.Match method

**Approved** | [#runtime/39443](https://github.com/dotnet/runtime/issues/39443#issuecomment-776164437) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h43m21s)

* Let's return the raw `List<T>` because it's more useful to callers (and we likely can't change the instance anyway)
* Let's rename the tuple fields to `Matched` and `Unmatched` to make easier to understand
* We should rename the method to `Match`

```C#
namespace System.Linq
{
    public partial static class Enumerable
    {
        public static (List<TSource> Matched, List<TSource> Unmatched) Match<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);
    }
}
```

## Add IsIPv6UniqueLocal to IPAddress

**Approved** | [#runtime/45148](https://github.com/dotnet/runtime/issues/45148#issuecomment-776169854) | [Video](https://www.youtube.com/watch?v=7bRCbwE9CYE&t=0h52m22s)

* We should remove the `Address` suffix

```C#
namespace System.Net
{
    public partial class IPAddress
    {
        // Existing:
        // public bool IsIPv4MappedToIPv6 { get; }
        // public bool IsIPv6LinkLocal { get; }
        // public bool IsIPv6Multicast { get; }
        // public bool IsIPv6SiteLocal { get; }
        // public bool IsIPv6Teredo { get; }
        public bool IsIPv6UniqueLocal { get; }
    }
}
```

