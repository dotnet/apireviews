# API Review 03/16/2023

## Obsolete JsonSerializerOptions.AddContext

**Approved** | [dotnet/runtime#83280](https://github.com/dotnet/runtime/issues/83280#issuecomment-1472379622)

Looks good as proposed.

Confirm with @terrajobst that System.Text.Json should use SYSLIB codes.

## Add ability to get modern Windows icons to System.Drawing.SystemIcons.

**Approved** | [dotnet/winforms#8802](https://github.com/dotnet/winforms/issues/8802#issuecomment-1472399939)

We added a `None=0` value to `StockIconOptions`.

Assuming that the shorter overload is passing `StockIconOptions.None`, consider collapsing into one overload with a defaulted parameter.

Consider renaming `StockIcon` to `StockIconId`, just in case there would be a future situation warranting something like `public class StockIcon : Icon`.

```C#
namespace System.Drawing;

public static class SystemIcons
{
    /// <summary>
    ///  Gets the specified stock shell icon.
    /// </summary>
    public static Icon GetStockIcon(StockIcon stockIcon);
    public static Icon GetStockIcon(StockIcon stockIcon, StockIconOptions options);
}

[Flags]
public enum StockIconOptions
{
    None = 0,
    SmallIcon = 0x000000001,
    ShellIconSize = 0x000000004,
    LinkOverlay = 0x000008000,
    Selected = 0x000010000,
}

// Matching https://learn.microsoft.com/windows/win32/api/shellapi/ne-shellapi-shstockiconid with
// Pascal casing and no abbreviations ("Assoc" to "Associations"), will still keep acronyms (CD, not CompactDisc).
public enum StockIcon
{
    DocumentNoAssociation = 0,
    DocumentWithAssociation = 1,
    Application = 2,
    Folder = 3,
    OpenFolder = 4,
    Drive525 = 5,
    Drive35 = 6,
    DriveRemovable = 7,
    DriveFixed = 8,
    DriveNetwork = 9,
    DriveNetworkDisabled = 10,
    DriveCD = 11,
    DriveRAM = 12,
    World = 13,
    Server = 15,
    Printer = 16,
    // ...
}
```

## Add ReadOnlySpan overloads to System.Drawing.Graphics

**Approved** | [#winforms/8844](https://github.com/dotnet/winforms/issues/8844#issuecomment-1472405556)

Assuming that all the underlying APIs take a (w)char and separate length (vs expecting null termination), looks good as proposed.

```C#
namespace System.Drawing
{
    public sealed partial class Graphics : System.MarshalByRefObject, System.Drawing.IDeviceContext, System.IDisposable
    {
        // Existing:
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.PointF point) { }
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.PointF point, System.Drawing.StringFormat? format) { }
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.RectangleF layoutRectangle) { }
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.RectangleF layoutRectangle, System.Drawing.StringFormat? format) { }
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, float x, float y) { }
        public void DrawString(string? s, System.Drawing.Font font, System.Drawing.Brush brush, float x, float y, System.Drawing.StringFormat? format) { }
        public System.Drawing.Region[] MeasureCharacterRanges(string? text, System.Drawing.Font font, System.Drawing.RectangleF layoutRect, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, System.Drawing.PointF origin, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, System.Drawing.SizeF layoutArea) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, System.Drawing.SizeF layoutArea, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, System.Drawing.SizeF layoutArea, System.Drawing.StringFormat? stringFormat, out int charactersFitted, out int linesFilled) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, int width) { throw null; }
        public System.Drawing.SizeF MeasureString(string? text, System.Drawing.Font font, int width, System.Drawing.StringFormat? format) { throw null; }

        // Proposal
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.PointF point) { }
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.PointF point, System.Drawing.StringFormat? format) { }
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.RectangleF layoutRectangle) { }
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, System.Drawing.RectangleF layoutRectangle, System.Drawing.StringFormat? format) { }
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, float x, float y) { }
        public void DrawString(ReadOnlySpan<char> s, System.Drawing.Font font, System.Drawing.Brush brush, float x, float y, System.Drawing.StringFormat? format) { }
        public System.Drawing.Region[] MeasureCharacterRanges(ReadOnlySpan<char> text, System.Drawing.Font font, System.Drawing.RectangleF layoutRect, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, System.Drawing.PointF origin, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, System.Drawing.SizeF layoutArea) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, System.Drawing.SizeF layoutArea, System.Drawing.StringFormat? stringFormat) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, System.Drawing.SizeF layoutArea, System.Drawing.StringFormat? stringFormat, out int charactersFitted, out int linesFilled) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, int width) { throw null; }
        public System.Drawing.SizeF MeasureString(ReadOnlySpan<char> text, System.Drawing.Font font, int width, System.Drawing.StringFormat? format) { throw null; }
    }

    public sealed partial class GraphicsPath : System.MarshalByRefObject, System.ICloneable, System.IDisposable
    {
        // Existing:
        public void AddString(string s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.Point origin, System.Drawing.StringFormat? format) { }
        public void AddString(string s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.PointF origin, System.Drawing.StringFormat? format) { }
        public void AddString(string s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.Rectangle layoutRect, System.Drawing.StringFormat? format) { }
        public void AddString(string s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.RectangleF layoutRect, System.Drawing.StringFormat? format) { }

        // Proposal
        public void AddString(ReadOnlySpan<char> s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.Point origin, System.Drawing.StringFormat? format) { }
        public void AddString(ReadOnlySpan<char> s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.PointF origin, System.Drawing.StringFormat? format) { }
        public void AddString(ReadOnlySpan<char> s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.Rectangle layoutRect, System.Drawing.StringFormat? format) { }
        public void AddString(ReadOnlySpan<char> s, System.Drawing.FontFamily family, int style, float emSize, System.Drawing.RectangleF layoutRect, System.Drawing.StringFormat? format) { }
    }
}
```

## Support modifying already initialized properties and fields when deserializing JSON

**Approved** | [#runtime/78556](https://github.com/dotnet/runtime/issues/78556#issuecomment-1472518084) | [Video](https://www.youtube.com/watch?v=2Z7MxaxQj0w&t=0h0m0s)

* Didn't realize it's a tiny change; @eiriktsarpalis represented it. Looks good as proposed.

```C#
namespace System.Text.Json.Serialization;

public enum JsonObjectCreationHandling
{
    Replace = 0,
    Populate = 1,
}

// NOTE: System.AttributeTargets.Class | System.AttributeTargets.Struct | System.AttributeTargets.Interface
//           For Property/Field - Populate is enforced
//           For Class/Struct/Interface - Populate is applied to properties where applicable
[AttributeUsage(
  AttributeTargets.Field | AttributeTargets.Property | AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Interface,
  AllowMultiple = false)]
public sealed partial class JsonObjectCreationHandlingAttribute : System.Text.Json.Serialization.JsonAttribute
{
    public JsonObjectCreationHandlingAttribute(System.Text.Json.Serialization.JsonObjectCreationHandling handling) { }
    public System.Text.Json.Serialization.JsonObjectCreationHandling Handling { get { throw null; } }
}

namespace System.Text.Json.Serialization.Metadata;

public abstract partial class JsonPropertyInfo
{
    public JsonObjectCreationHandling? ObjectCreationHandling { get; set; }
}

public abstract partial class JsonTypeInfo
{
    public JsonObjectCreationHandling? PreferredPropertyObjectCreationHandling { get; set; }
}


namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public JsonObjectCreationHandling PreferredObjectCreationHandling { get; set; } /*= JsonObjectCreationHandling.Replace; */
}
```

## IUtf8SpanFormattable and IUtf8SpanParsable

**Approved** | [#runtime/81500](https://github.com/dotnet/runtime/issues/81500#issuecomment-1472608562) | [Video](https://www.youtube.com/watch?v=2Z7MxaxQj0w&t=0h14m29s)

* We should add `utf8` in the parameter names
* We should use `ReadOnlySpan<char>` as the format to aid with composition with compiler features around string formatting
    - @TannerGooding and @StephenToub will talk to the @dotnet/ldm

```C#
namespace System;

public interface IUtf8SpanFormattable
{
    bool TryFormat(Span<byte> utf8Destination, out int bytesWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
}

public interface IUtf8SpanParsable<TSelf>
    where TSelf : IUtf8SpanParsable<TSelf>?
{
    static abstract TSelf Parse(ReadOnlySpan<byte> utf8, IFormatProvider? provider);
    static abstract bool TryParse(ReadOnlySpan<byte> utf8, IFormatProvider? provider, [MaybeNullWhen(returnValue: false)] out TSelf result);
}
```

```C#
namespace System.Numerics;

public interface INumberBase<TSelf>
{
    static virtual TSelf Parse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider);
    static virtual bool TryParse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result);
}
```
