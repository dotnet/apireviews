# API Review 09/14/2021

## UriCreationOptions

**Approved** | [#runtime/59099](https://github.com/dotnet/runtime/issues/59099#issuecomment-919412394) | [Video](https://www.youtube.com/watch?v=0UAoOvgn_7g&t=0h0m0s)

* The struct should be a mutable struct
    - We should pass it in via `in`, which means we should mark the getter as `readonly`
* `GetComponents()` should throw `InvaliOperationException`
    - Note: even `UriFormat.Unescaped` does some transformations, so this also doesn't make sense to allow

```C#
namespace System
{
    public struct UriCreationOptions
    {
        public UriKind UriKind { readonly get; set; }
        public bool AllowImplicitFilePaths { readonly get; set; }
        public bool DangerousDisablePathAndQueryCanonicalization { readonly get; set; }
    }

    public partial class Uri
    {
        // Existing
        // public Uri(string uriString);
        // public Uri(string uriString, UriKind uriKind);
        public Uri(string uriString, in UriCreationOptions creationOptions);
        
        // Existing
        // public static bool TryCreate(string? uriString, UriKind uriKind, out Uri? result);
        public static bool TryCreate(string? uriString, in UriCreationOptions creationOptions, out Uri? result);
    }
}
```
## Compile-time source generation for System.Text.Json

**Approved** | [#runtime/45448](https://github.com/dotnet/runtime/issues/45448#issuecomment-919421589) | [Video](https://www.youtube.com/watch?v=0UAoOvgn_7g&t=0h31m7s)

* Looks good as proposed

```diff
namespace System.Text.Json.Serialization.Metadata
{
    // Provides serialization info about a type
    public abstract partial class JsonTypeInfo<T> : JsonTypeInfo
    {
        internal JsonTypeInfo();
-       public Action<Utf8JsonWriter, T>? Serialize { get; }
+       // Rename to SerializeHandler
+       public Action<Utf8JsonWriter, T>? SerializeHandler { get; }
    }

    public static partial class JsonMetadataServices
    {
+       // Converter that handles JsonArray
+       public static JsonConverter<JsonArray> JsonArrayConverter { get; }

+       // Converter that handles JsonNode
+       public static JsonConverter<JsonNode> JsonNodeConverter { get; }

+       // Converter that handles JsonObject
+       public static JsonConverter<JsonObject> JsonObjectConverter { get; }

+       // Converter that handles JsonValue
+       public static JsonConverter<JsonValue> JsonValueConverter { get; }

+       // Converter that handles unsupported types.
+       public static JsonConverter<T> GetUnsupportedTypeConverter<T>();
    }
}
```
## System.Numerics.Vector2.Equals doesn't use float.Equals in Equals implementation 

**Approved** | [#runtime/58853](https://github.com/dotnet/runtime/issues/58853#issuecomment-919426955) | [Video](https://www.youtube.com/watch?v=0UAoOvgn_7g&t=0h43m19s)

* We're OK with taking this breaking change for .NET 7.0
* Should we add an analyzer that flags usages of `==` when comparing floating point data types in the body of an `Equals` method?
## [RegexGenerator(...)] attribute

**Approved** | [#runtime/58880](https://github.com/dotnet/runtime/issues/58880#issuecomment-919438549) | [Video](https://www.youtube.com/watch?v=0UAoOvgn_7g&t=0h50m41s)

* Looks good as proposed
    - We should rename `matchTimeout` to `matchTimeoutMilliseconds` as we can't use a `TimeSpan`

```C#
namespace System.Text.RegularExpressions
{
    [AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
    public sealed class RegexGeneratorAttribute : Attribute
    {
        public RegexGeneratorAttribute(string pattern);
        public RegexGeneratorAttribute(string pattern, RegexOptions options);
        public RegexGeneratorAttribute(string pattern, RegexOptions options, int matchTimeoutMilliseconds);

        public string Pattern { get; }
        public RegexOptions Options { get; }
        public int MatchTimeoutMilliseconds { get; }
    }
}
```
