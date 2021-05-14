# Quick Reviews 05/14/2021

## Add new option bag for FileStream ctor

**Approved** | [#runtime/52446](https://github.com/dotnet/runtime/issues/52446#issuecomment-841408090)

* There is no notion of required properties for structs/classes
    - We should extract path and keep it as a constructor parameter
    - IOW, we the options type should focus on optional parameters
* Does this need to be a struct? Structs always have usability issues and this API seems like the one where the constructor does so much work that the allocation of the options type doesn't seem to matter.
    - Is there value in `init` or is `get`/`set` good enough? The benefit is wider language support and presumably `FileStream` will copy the values anyways because there are already fields and/or the options are directly translated into the native code without holding onto them.
* We should have a separate issue about the ability to pass in the buffer as that opens up the possibility of use-after-free issues and also makes it more error prone to share options across multiple `FileStream` instances
* `PreAllocation`
    - Should this be `Preallocation` or `PreAllocation`? @bartonjs to decide.
* The options don't cover the constructor that takes a handle. Should it?

```C#
namespace System.IO
{
    public sealed class FileStreamOptions
    {
        public FileStreamOptions();
        public FileMode Mode { get; set; }
        public FileAccess Access { get; set; }
        public FileShare Share { get; set; }
        public FileOptions Options { get; set; }
        public long PreAllocationSize { get; set; }
    }
    public partial class FileStream : Stream
    {
        // Existing:
        // public FileStream(string path, FileMode mode)
        // public FileStream(string path, FileMode mode, FileAccess access)
        // public FileStream(string path, FileMode mode, FileAccess access, FileShare share)
        // public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize)
        // public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize, bool useAsync)
        // public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize, FileOptions options)
        public FileStream(string path, FileStreamOptions options);

        // Hiding obsoleting APIs

        [EditorBrowsable(EditorBrowsableState.Never)]
        public FileStream(IntPtr handle, FileAccess access);

        [EditorBrowsable(EditorBrowsableState.Never)]
        public FileStream(IntPtr handle, FileAccess access, bool ownsHandle);

        [EditorBrowsable(EditorBrowsableState.Never)]
        public FileStream(IntPtr handle, FileAccess access, bool ownsHandle, int bufferSize);

        [EditorBrowsable(EditorBrowsableState.Never)]
        public FileStream(IntPtr handle, FileAccess access, bool ownsHandle, int bufferSize, bool isAsync);
    }
}
```

## Provide fast-path serialization logic in JSON source generator

**NeedsWork** | [#runtime/51945](https://github.com/dotnet/runtime/issues/51945#issuecomment-841456779)

* Instead of generating a single `JsonContext` type per assembly, we should let the user define the `JsonContext` type (controlling namespace, type name, etc.) and apply the assembly level attributes to this type and have the generator fill in the body of the body. This allows the user to have multiple different JSON options for different types in a single assembly, e.g.
    - `Fabrikam.Telemetry.TelemetryJsonContext`
    - `Fabrikam.Shopping.ShoppingJsonContext`
* We should rename `JsonSerializerOptionsAttribute` the name implies that it's related to `JsonSerializerOptions` but this describes the runtime serialization options while this attributes the static, source generator-based options that are similar, but generally disjoint because of the nature of needing to be statically describable (e.g. no converters, no polymorphic naming conventions etc).
* In fact, we should consider taking all the attributes and base types that are relevant to source-generated JSON serializer into a dedicated namespace (e.g. `System.Text.Json.Serialization.Generator` or `System.Text.Json.Serialization.Metadata`).


```C#
namespace System.Text.Json.Serialization.Metadata
{
    public abstract partial class JsonTypeInfo<T>
    {
        // Existing:
        // internal JsonTypeInfo() { }
        public Action<Utf8JsonWriter, T>? Serialize { get; set; }
    }
    [AttributeUsage(AttributeTargets.Assembly, AllowMultiple = false)]
    public class JsonSerializerOptionsAttribute : JsonAttribute
    {
        public JsonIgnoreCondition DefaultIgnoreCondition { get; set; }
        public bool IgnoreReadOnlyFields { get; set; }
        public bool IgnoreReadOnlyProperties { get; set; }
        public bool IgnoreRuntimeCustomConverters { get; set; }
        public bool IncludeFields { get; set; }
        public JsonKnownNamingPolicy NamingPolicy { get; set; }
        public bool WriteIndented { get; set; }
    }
}
namespace System.Text.Json.Serialization
{
    // Existing: [AttributeUsage(AttributeTargets.Assembly, AllowMultiple = true)]
    [AttributeUsage(AttributeTargets.Assembly |
                    AttributeTargets.Class |
                    AttributeTargets.Struct |
                    AttributeTargets.Interface, AllowMultiple = true)]
    public partial class JsonSerializableAttribute : Attribute
    {
        public JsonSourceGenerationMode GenerationMode { get; set; }
    }
    public enum JsonKnownNamingPolicy
    {
        Unspecified = 0,
        BuiltInCamelCase = 1
    }
    public enum JsonSourceGenerationMode
    {
        MetadataAndSerialization = 0,
        Metadata = 1,
        Serialization = 2
    }
}
```

