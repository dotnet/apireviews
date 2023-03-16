# API Review 03/16/2023

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
