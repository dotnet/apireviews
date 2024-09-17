# API Review 09/17/2024

## Codify that String.Create can write over the string's null terminator

**Approved** | [#runtime/106903](https://github.com/dotnet/runtime/issues/106903#issuecomment-2356488882) | [Video](https://www.youtube.com/watch?v=uyXehmcAkHc&t=0h0m0s)

* We're good at guaranteeing that the span points to memory that has space for a least one more element than the span
* We'd guarantee that the span points to zero'd memory, including the null terminator.
* Our docs would say that if you write to the extra location it must be a null terminator. Writing anything else will result corrupting the string and thus is undefined behavior.

## Add DynamicallyAccessedMemberTypes.AllMethods/.AllFields/.AllEvents/etc

**Approved** | [#runtime/88512](https://github.com/dotnet/runtime/issues/88512#issuecomment-2356586144) | [Video](https://www.youtube.com/watch?v=uyXehmcAkHc&t=0h9m31s)

* We have different values for constructors, fields, methods, such that a single a single place can combine different kinds (methods, files etc) where some include the entire hierarchy while others do not. For example, we don't want  `AllConstructors | PublicMethods | NonPublicMethods` to imply `AllMethods`.


```C#
namespace System.Diagnostics.CodeAnalysis;

public enum DynamicallyAccessedMemberTypes
{
    // Existing members
    // ...

    // New members:

    PublicConstructorsWithInherited = PublicConstructors | 0x100000,
    PublicNestedTypesWithInherited = PublicNestedTypes | 0x200000,

    NonPublicConstructorsWithInherited = NonPublicConstructors | 0x4000,
    NonPublicMethodsWithInherited = NonPublicMethods | 0x8000,
    NonPublicFieldsWithInherited = NonPublicFields | 0x10000,
    NonPublicNestedTypesWithInherited = NonPublicNestedTypes | 0x20000,
    NonPublicPropertiesWithInherited = NonPublicProperties | 0x40000,
    NonPublicEventsWithInherited = NonPublicEvents | 0x80000,

    AllConstructors = PublicConstructorsWithInherited | NonPublicConstructorsWithInherited,
    AllMethods = PublicMethods | NonPublicMethodsWithInherited,
    AllFields = PublicFields | NonPublicFieldsWithInherited,
    AllNestedTypes = PublicNestedTypesWithInherited | NonPublicNestedTypesWithInherited,
    AllProperties = PublicProperties | NonPublicPropertiesWithInherited,
    AllEvents = PublicEvents | NonPublicEventsWithInherited,
}
```

## Enumerable.Reverse<T>(T[])

**Approved** | [#runtime/107723](https://github.com/dotnet/runtime/issues/107723#issuecomment-2356602408) | [Video](https://www.youtube.com/watch?v=uyXehmcAkHc&t=0h57m32s)

* As a rare tie breaker this is fine.

```C#
namespace System.Linq;

public static class Enumerable
{
    public static IEnumerable<TSource> Reverse<TSource>(this TSource[] source);
}
```
## Add JsonMarshal.GetRawUtf8PropertyName API

**Approved** | [#runtime/107782](https://github.com/dotnet/runtime/issues/107782#issuecomment-2356610191) | [Video](https://www.youtube.com/watch?v=uyXehmcAkHc&t=1h6m41s)

* Looks good as proposed

```C#
namespace System.Runtime.InteropServices;

public static partial class JsonMarshal
{
    // Existing API
    // public static ReadOnlySpan<byte> GetRawUtf8Value(JsonElement element);

    // New API
    public static ReadOnlySpan<byte> GetRawUtf8PropertyName(JsonProperty property);
}
```

## JsonSourceGenerationOptions should support ReferenceHandler

**Approved** | [#runtime/107597](https://github.com/dotnet/runtime/issues/107597#issuecomment-2356620566) | [Video](https://www.youtube.com/watch?v=uyXehmcAkHc&t=1h11m18s)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization;

public enum KnownReferenceHandler // Naming follows the ReferenceHandler type which misses a Json prefix
{ 
    IgnoreCycles = 1,
    Preserve = 2,
    Unspecified = 0,
}

public partial class JsonSourceGenerationOptionsAttribute
{
    public KnownReferenceHandler ReferenceHandler { get; set; }
}
```
