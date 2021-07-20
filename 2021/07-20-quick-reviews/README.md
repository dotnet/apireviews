# API Review 07/20/2021

## Introduce ApplyApplicationDefaults Event to the Application Framework

**Approved** | [#winforms/5069](https://github.com/dotnet/winforms/issues/5069#issuecomment-883569659) | [Video](https://www.youtube.com/watch?v=POU7RxyBAoI&t=0h0m0s)

API Review:

* `ApplyApplicationDefaultsEventHandler` should be `EventHandler<ApplyApplicationDefaults>`, but since the rest of the events on this type are custom delegates doing this one that way is OK (and may be needed due to an IDE limitation)
* Ideally `MinimumSplashScreenDisplayTime` would be a `TimeSpan`, but since it's just echoing an existing unitless `int` property it's OK.

```C#
namespace Microsoft.VisualBasic.ApplicationServices
{
    public partial class WindowsFormsApplicationBase
    {
        public event EventHandler<ApplyDefaultsEventArgs> ApplyApplicationDefaults;
        
        [EditorBrowsable(EditorBrowsableState.Never)]
        protected HighDpiMode HighDpiMode { get; set; }

        // Existing property, but new attribute
        [EditorBrowsable(EditorBrowsableState.Never)]
        public int MinimumSplashScreenDisplayTime { get; set; }
    }

    public class ApplyApplicationDefaultsEventArgs : EventArgs
    {
        public Font DefaultFont { get; set; }
        public HighDpiMode HighDpiMode { get; set; }
        public int MinimumSplashScreenDisplayTime { get; set; }
    }

    [EditorBrowsable(EditorBrowsableState.Advanced)]
    public delegate void ApplyApplicationDefaultsEventHandler(object sender, ApplyDefaultsEventArgs e);
}
```
## We should be able serialize and deserialize from DOM

**Approved** | [#runtime/31274](https://github.com/dotnet/runtime/issues/31274#issuecomment-883578067) | [Video](https://www.youtube.com/watch?v=POU7RxyBAoI&t=0h28m29s)

Looks good as proposed.

```C#
namespace System.Text.Json
{
    public static partial class JsonSerializer
    {
	       public static TValue? Deserialize<TValue>(this JsonDocument document, JsonSerializerOptions? options = null);
	       public static object? Deserialize(this JsonDocument document, Type returnType, JsonSerializerOptions? options = null);
	       public static TValue? Deserialize<TValue>(this JsonDocument document, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static object? Deserialize(this JsonDocument document, Type returnType, JsonSerializerContext context);
	  
	       public static JsonDocument SerializeToDocument<TValue>(TValue value, JsonSerializerOptions? options = null);
	       public static JsonDocument SerializeToDocument(object? value, Type inputType, JsonSerializerOptions? options = null);
	       public static JsonDocument SerializeToDocument<TValue>(TValue value, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static JsonDocument SerializeToDocument(object? value, Type inputType, JsonSerializerContext context);
	  
	       public static TValue? Deserialize<TValue>(this JsonElement element, JsonSerializerOptions? options = null);
	       public static TValue? Deserialize<TValue>(this JsonElement element, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static object? Deserialize(this JsonElement element, Type returnType, JsonSerializerOptions? options = null);
	       public static object? Deserialize(this JsonElement element, Type returnType, JsonSerializerContext context);
	  
	       public static JsonElement SerializeToElement<TValue>(TValue value, JsonSerializerOptions? options = null);
	       public static JsonElement SerializeToElement(object? value, Type inputType, JsonSerializerOptions? options = null);
	       public static JsonElement SerializeToElement<TValue>(TValue value, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static JsonElement SerializeToElement(object? value, Type inputType, JsonSerializerContext context);
	  
	       public static TValue? Deserialize<TValue>(this JsonNode node, JsonSerializerOptions? options = null);
	       public static TValue? Deserialize<TValue>(this JsonNode node, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static object? Deserialize(this JsonNode node, Type returnType, JsonSerializerOptions? options = null);
	       public static object? Deserialize(this JsonNode node, Type returnType, JsonSerializerContext context);
	  
	       public static JsonNode? SerializeToNode<TValue>(TValue value, JsonSerializerOptions? options = null);
	       public static JsonNode? SerializeToNode(object? value, Type inputType, JsonSerializerOptions? options = null);
	       public static JsonNode? SerializeToNode<TValue>(TValue value, JsonTypeInfo<TValue> jsonTypeInfo);
	       public static JsonNode? SerializeToNode(object? value, Type inputType, JsonSerializerContext context);
    }
}
```
## Add blittable Color to System.Numerics

**NeedsWork** | [#runtime/48615](https://github.com/dotnet/runtime/issues/48615#issuecomment-883635884) | [Video](https://www.youtube.com/watch?v=POU7RxyBAoI&t=0h41m34s)

We lost quorum during the API Review meeting for this.  There was concern that it's proposed as a primitive and there's not clear evidence that the interested parties have signed off that it's usable for them.  So it'd be great to get that feedback from someone before approval.

Notes we took when discussing the proposal:


* The types should be ISpanFormattable
* The constraints on these types would ideally be `IScalar`.  If we can get the constraint more constrained that would be great.
* They're IFormattable, should they be IParsable?
* The Argb and Rgba non-generic types should also have BigEndian versions of their routines.
* The System.Drawing.Color from `Argb<byte>` should be an overload of `Color.FromArgb`
* Eliminate the RGBA interaction to System.Drawing.Color in favor of conversions from RGBA to ARGB
  * Argb<T>.ToRgba<T>, and Rgba<T>.ToArgb<T>.  Don't need the Froms.

```C#
namespace System.Drawing
{
    partial struct Color : IEquatable<Color>
    {
        public static Color FromArgb(System.Numerics.Colors.Argb<byte> argb);
        public static implicit operator Color(System.Numerics.Colors.Argb<byte> argb);

        // ToNumericsArgb? ToArgbNumerics?
        public System.Numerics.Colors.Argb<byte> ToArgbValue();
        public static explicit operator System.Numerics.Colors.Argb<byte>(in Color color);
    }
}

namespace System.Numerics.Colors
{
    public static class Argb
    {
        public static Argb<byte> CreateBigEndian(uint color);
        public static Argb<byte> CreateLittleEndian(uint color);
        public static uint ToUInt32LittleEndian(this Argb<byte> color);
        public static uint ToUInt32BigEndian(this Argb<byte> color);
    }

    public readonly struct Argb<T> : IEquatable<Argb<T>>, IFormattable, ISpanFormattable where T : struct
    {
        public T A { get; }
        public T R { get; }
        public T G { get; }
        public T B { get; }

        public Argb(T a, T r, T g, T b);
        public Argb(ReadOnlySpan<T> values);
        public void CopyTo(Span<T> destination);
        public bool Equals(Argb<T> other);
        public string ToString(string format, IFormatProvider formatProvider);
        // whatever ISpanFormattable says
        public Rgba<T> ToRgba();
    }

    public static class Rgba
    {
        public static Rgba<byte> CreateLittleEndian(uint color);
        public static Rgba<byte> CreateBigEndian(uint color);
        public static uint ToUInt32LittleEndian(this Rgba<byte> color);
        public static uint ToUInt32BigEndian(this Rgba<byte> color);
    }

    public readonly struct Rgba<T> : IEquatable<Rgba<T>>, IFormattable, ISpanFormattable where T : struct
    {
        public T R { get; }
        public T G { get; }
        public T B { get; }
        public T A { get; }

        public Rgba(T r, T g, T b, T a);
        public Rgba(ReadOnlySpan<T> values);
        public void CopyTo(Span<T> destination);
        public bool Equals(Rgba<T> other);
        public string ToString(string format, IFormatProvider formatProvider);
        // whatever ISpanFormattable says
        public Argb<T> ToArgb();
    }
}
```

