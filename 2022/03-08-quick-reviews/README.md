# API Review 03/08/2022

## Let consumers of MemoryCache access metrics

**Approved** | [#runtime/50406](https://github.com/dotnet/runtime/issues/50406#issuecomment-1062073606) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=0h0m0s)

* We should wait until we have evidence that anyone needs them on the interface
* If there is, then we'd prefer a pattern like `IMemoryCache2 : IMemoryCache` such that we always have a latest version that has all the features, rather than a permutation of all possible implementations. DIMs would be another approach.

```C#
namespace Microsoft.Extensions.Caching.Memory
{
    // interface IMemoryCacheHasStats
    // {
    //     MemoryCacheStatistics GetCurrentStatistics();
    // }   
    public partial class MemoryCache /* : IMemoryCacheHasStats */
    {
        public MemoryCacheStatistics GetCurrentStatistics();
    }
    public partial readonly struct MemoryCacheStatistics
    {
        public long TotalRequests { get; init; }
        public long TotalHits { get; init; }
        public long CurrentEntryCount { get; init; }
        public long? CurrentSize { get; init; }
    }    
}
```
## Specify supported custom type marshalling features on CustomTypeMarshallerAttribute

**Approved** | [#runtime/66121](https://github.com/dotnet/runtime/issues/66121#issuecomment-1062084220) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=0h21m32s)

* We should add a zero-value for `CustomTypeMarshallerDirection` such that default initialized values have an associated member. We should call it `None` and simply reject this as invalid for `CustomTypeMarshallerAttribute.Direction`. We should hide it.

```C#
namespace System.Runtime.InteropServices
{
    public partial class CustomTypeMarshallerAttribute : Attribute
    {
        public CustomTypeMarshallerDirection Direction { get; set; } = CustomTypeMarshallerDirection.Ref;
        public CustomTypeMarshallerFeatures Features { get; set; }
    }
    [Flags]
    public enum CustomTypeMarshallerDirection
    {
        [EditorBrowsable(EditorBrowsableState.Never)]
        None = 0x0,
        In = 0x1,
        Out = 0x2,
        Ref = CustomTypeMarshallerDirection.In | CustomTypeMarshallerDirection.Out
    }
    [Flags]
    public enum CustomTypeMarshallerFeatures
    {
        None = 0x0,
        UnmanagedResources = 0x1,
        CallerAllocatedBuffer = 0x2,
        TwoStageMarshalling = 0x4,
    }
}
```
## Add blittable Color to System.Numerics

**Approved** | [#runtime/48615](https://github.com/dotnet/runtime/issues/48615#issuecomment-1062091679) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=0h34m1s)

* Looks good as proposed

```C#
using System;

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

// WPF
namespace System.Windows.Media
{
    public struct Color : IEquatable<Color>, IFormattable
    {
        public static Color FromArgb(System.Numerics.Colors.Argb<byte> argb);
        public static implicit operator Color(System.Numerics.Colors.Argb<byte> argb);
        public static Color FromArgb(System.Numerics.Colors.Argb<float> argb);
        public static implicit operator Color(System.Numerics.Colors.Argb<float> argb);

        public System.Numerics.Colors.Argb<byte> ToArgbValue();
        public static explicit operator System.Numerics.Colors.Argb<byte>(in Color color);
        public System.Numerics.Colors.Argb<float> ToArgbValue();
        public static explicit operator System.Numerics.Colors.Argb<float>(in Color color);
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
## Additional JsonNode functionality

**Approved** | [#runtime/56592](https://github.com/dotnet/runtime/issues/56592#issuecomment-1062114464) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=0h43m39s)

* `XDocument` uses a copy constructor on `XElement` instead, but `DeepClone` seems more discoverable.
* `IsRoot` doesn't seem that value, as the canonical way would be to check `node.Parent is null`.
* `GetValueKind()` should try to return a better value than `Undefined` such that modifications of primitives doesn't result in unexpected changes from, say, `Number` to `Undefined`.
    - Do we need the options? We might need it for settings such as the one that serializes numbers as strings
* In `XDocument` the `Replace` method is called `ReplaceWith`, which makes it a bit more clear that it's in-place semantics
* We need to define what `JsonArray.GetValues<object>` will do. Return the `JsonNode` seems the most viable, because boxing the underlying primitives wouldn't be well-defined.

```C#
namespace System.Text.Json.Nodes
{
     public partial class JsonNode
     {
        public JsonNode DeepClone();
        public static bool DeepEquals(JsonNode? node1, JsonNode? node2);
        public JsonValueKind GetValueKind(JsonSerializerOptions options = null);
        public string GetPropertyName();
        public int GetElementIndex();
        public void ReplaceWith<T>(T value);
    }
    public partial class JsonArray
    {
        public IEnumerable<T> GetValues<T>();
    }
}
```
## Stopwatch.GetElapsedTime

**Approved** | [#runtime/65858](https://github.com/dotnet/runtime/issues/65858#issuecomment-1062118260) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=1h12m29s)

* Looks good as proposed

```C#
namespace System.Diagnostics
{
    public partial class Stopwatch
    {
        public static TimeSpan GetElapsedTime(long startingTimestamp);
        public static TimeSpan GetElapsedTime(long startingTimestamp, long endingTimestamp);
    }
}
```
## Activity.HasRemoteParent (and problematic docs for Activity.Parent)

**Approved** | [#runtime/65660](https://github.com/dotnet/runtime/issues/65660#issuecomment-1062124497) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=1h17m17s)

* Looks good as proposed

```C#
namespace System.Diagnostics
{
    public partial class Activity
    {
        public bool HasRemoteParent { get; }
    }
}
```
## Add method TryFormat for Enum

**Approved** | [#runtime/57881](https://github.com/dotnet/runtime/issues/57881#issuecomment-1062129899) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=1h24m48s)

* Makes sense
* It seems also desirable to also implement `ISpanFormattable`, this would be useful, for example, for interpolated strings
    - This would require work in the JIT to avoid the boxing overhead

```C#
namespace System
{
    public partial class Enum : ISpanFormattable
    {
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider? provider);

        public static bool TryFormat<TEnum>(TEnum value, Span<char> destination, out int charsWritten) 
             where TEnum : struct, Enum;
     }
}
```
## Scale property for the decimal datatype

**Approved** | [#runtime/65074](https://github.com/dotnet/runtime/issues/65074#issuecomment-1062132366) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=1h31m15s)

* Looks good as proposed

```C#
namespace System
{
    public partial struct Decimal
    {
        public byte Scale { get; }
    }
}
```
## System.Runtime.CompilerServices.CompilerFeatureRequiredAttribute

**NeedsWork** | [#runtime/66167](https://github.com/dotnet/runtime/issues/66167#issuecomment-1062143475) | [Video](https://www.youtube.com/watch?v=0nixAM-u2Fw&t=1h35m14s)

* Makes sense
* It's not clear why the `Language` property exists; it seems it's geared towards expressing optionality. But if other languages later also support the feature, would they emit the attribute using their language or would the emit with the name of the first language?

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.All, AllowMultiple = true, Inherited = false)]
    public sealed class CompilerFeatureRequiredAttribute : Attribute
    {
        public CompilerFeatureRequiredAttribute(string featureName)
        {
            FeatureName = featureName;
        }
        public string FeatureName { get; }
        public string? Language { get; init; }
    }
}
```
