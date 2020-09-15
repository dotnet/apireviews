# Quick Reviews 09/15/2020

## System.Net.Http.HttpRequestException is not marked as [Serializable]

**Approved** | [#runtime/42154](https://github.com/dotnet/runtime/issues/42154#issuecomment-692890590) | [Video](https://www.youtube.com/watch?v=LpXyfXS8p70&t=0h0m0s)

* Seems fine per our guidance
* We won't do this for all exception, but only for exceptions that exist in both .NET Framework and .NET Core
* Specifically, we won't do this for .NET Core-only exceptions
* And moving forward, we'll phase out `BinaryFormatter` anyways

```C#
namespace System.Net.Http
{
    [Serializable]
    public class HttpRequestException
    {
        protected HttpRequestException(SerializationInfo info, StreamingContext context);
        public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
}
```
## Add safe overloads to TextRenderer to get output strings.

**Approved** | [#winforms/3710](https://github.com/dotnet/winforms/issues/3710#issuecomment-692900947) | [Video](https://www.youtube.com/watch?v=LpXyfXS8p70&t=0h17m39s)

* Normally, output buffers go right after the input buffer, but we'll keep it last here to align with existing overloads and (more importantly) the `out` string

```C#
namespace System.Windows.Forms
{
    public sealed class TextRenderer
    {
        // Existing flags APIs:
        // public static void DrawText(IDeviceContext dc, string text, Font font, Point pt, Color foreColor, Color backColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, string text, Font font, Point pt, Color foreColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, string text, Font font, Rectangle bounds, Color foreColor, Color backColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, string text, Font font, Rectangle bounds, Color foreColor, TextFormatFlags flags);
        // public static Size MeasureText(IDeviceContext dc, string text, Font font, Size proposedSize, TextFormatFlags flags);
        // public static Size MeasureText(string text, Font font, Size proposedSize, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, Color backColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, Color backColor, TextFormatFlags flags);
        // public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, TextFormatFlags flags);
        // public static Size MeasureText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags);
        // public static Size MeasureText(ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags);

        public static void DrawText(IDeviceContext dc, string text, Font font, Point pt, Color foreColor, Color backColor, TextFormatFlags flags, out string outputText);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, Color backColor, TextFormatFlags flags, Span<char> outputTextBuffer, out int outputTextLength);
        public static void DrawText(IDeviceContext dc, string text, Font font, Rectangle bounds, Color foreColor, Color backColor, TextFormatFlags flags, out string outputText);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, Color backColor, TextFormatFlags flags, Span<char> outputTextBuffer, out int outputTextLength);
        public static Size MeasureText(IDeviceContext dc, string text, Font font, Size proposedSize, TextFormatFlags flags, out string outputText);
        public static Size MeasureText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags, Span<char> outputTextBuffer, out int outputTextLength);
        public static Size MeasureText(string text, Font font, Size proposedSize, TextFormatFlags flags, out string outputText);
        public static Size MeasureText(ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags, Span<char> outputTextBuffer, out int outputTextLength);
    }
}
```
