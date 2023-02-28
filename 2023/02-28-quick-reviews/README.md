# API Review 02/28/2023

## Support modifying already initialized properties and fields when deserializing JSON

**Approved** | [#runtime/78556](https://github.com/dotnet/runtime/issues/78556#issuecomment-1448725306)

* We renamed `JsonSerializerOptions.DefaultCreationHandling` to `PreferredObjectCreationHandling` both to indicate that the setting may have contextual fallbacks, and to keep "object" in the name so it doesn't sound like it applies to serializing
* We also added `Object` into `JsonPropertyInfo.CreationHandling` (now `ObjectCreationHandling`)
* Just `Handling` seems fine on `JsonObjectCreationHandlingAttribute`, and consistent with the other Json*Handling attribute/options.
* During the feature discussion we discussed "not allowed /for now/" on immutable arrays, and decided that they are not allowed for-ever, because of breaking change complexities.

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
    public JsonObjectCreationHandling ObjectCreationHandling { get; set; } // Replace remains default behavior
}

namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public JsonObjectCreationHandling PreferredObjectCreationHandling { get; set; } /*= JsonObjectCreationHandling.Replace; */
}
```
## [Analyzer] Adjust/simplify code for numeric IntPtr

**Approved** | [#runtime/74518](https://github.com/dotnet/runtime/issues/74518#issuecomment-1448742125)

Looks good as proposed. The only representative case that's not explicitly mentioned is `var nint = IntPtr.Zero`, which needs special handling because of var.
## Create array from array type

**Approved** | [#runtime/76478](https://github.com/dotnet/runtime/issues/76478#issuecomment-1448753932)

Looks good as proposed.

The implementer is encouraged to consider whether all of these overloads are actually required, as the suggestion that the first overload will be in excess of 99% of all calls seems correct.

```C#
namespace System;

public partial class Array
{
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, int length);
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, int length1, int length2);
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, int length1, int length2, int length3);
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, params int[] lengths);
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, int[] lengths, int[] lowerBounds);
        public static System.Array CreateInstanceFromArrayType(System.Type arrayType, params long[] lengths);
}
```
## `OperatingSystem.IsWasi()`

**Approved** | [#runtime/78389](https://github.com/dotnet/runtime/issues/78389#issuecomment-1448778796)

Looks good as proposed.

Most of the other OperatingSystem.Is* methods have a -VersionAtLeast variant (with varying numbers of accepted integers), consider if WASI needs one (in the future).

```C#
namespace System;

public sealed partial class OperatingSystem : ISerializable, ICloneable
{
     public static bool IsWasi();
}
```
## bool does not implement IParsable<TSelf>

**Approved** | [#runtime/78523](https://github.com/dotnet/runtime/issues/78523#issuecomment-1448791521)

Adding the interface decl on bool and implementing the methods explicitly seems reasonable, though we should go all the way to ISpanParsable

```C#
namespace System;

public partial struct Boolean : ISpanParsable<Boolean>
{
    // ISpanParsable and IParsable members explicitly implemented (if they're not already present)
}
```
## String should implement IParsable<string>

**Approved** | [#runtime/78842](https://github.com/dotnet/runtime/issues/78842#issuecomment-1448805002)

Going all the way to ISpanParsable seems legit.

Since the longest valid `ReadOnlySpan<char>` is longer than the longest valid `string`, the TryParse on a ROS-char should return false for a too-long input.

```C#
namespace System;

// These implementations use explicit interface implementation, so that the methods
// are not directly visible on string.
public partial class String : ISpanParsable<String>
{
    static string IParsable<String>.Parse(string s, IFormatProvider? provider) => s;
    // returns true if s is non-null, and sets result = s.
    static bool IParsable<String>.TryParse(string? s, IFormatProvider? provider, out string result);

    // also explicitly implement ISpanParsable
}

## MemoryExtensions.AsSpan(this string? text, Range range)

**Approved** | [#runtime/77955](https://github.com/dotnet/runtime/issues/77955#issuecomment-1448765789)

Since there's already an AsMemory overload with the same shape, this seems like it's just properly squaring off the feature.

Since `AsSpan(string?, Index)` isn't present, but `AsSpan(T[], Index)` and `AsSpan(ArraySegment<T>, Index)` do, we've added that to further square this off.

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static ReadOnlySpan<char> AsSpan(this string? text, Range range);
    public static ReadOnlySpan<char> AsSpan(this string? text, Index startIndex);
}
```
