# Quick Reviews 4/7/2020

## Consider exposing Socket(SafeSocketHandle fd) constructor

**Approved** | [#runtime/862](https://github.com/dotnet/runtime/issues/862#issuecomment-610520669) | [Video](https://www.youtube.com/watch?v=POdrqNOlY5g&t=0h0m0s)

Looks good as proposed:

```C#
namespace System.Net.Sockets
{
    public partial class Socket
    {
        public Socket(SafeSocketHandle handle);
    }
}
````

## MemoryMappedFile constructor

**Approved** | [#runtime/33206](https://github.com/dotnet/runtime/issues/33206#issuecomment-610526663) | [Video](https://www.youtube.com/watch?v=POdrqNOlY5g&t=0h26m35s)

* We're fine with taking the `handle` parameter but we have concerns with the `fileStream` parameter because it seems plumbing, not part of a sensible API.
  * Option 1: Move the implementation from the higher level assembly to the assembly containing `MemoryMappedFile` (while the ref assembly setup)
  * Option 2.1 "Smuggle" the file stream via the `SafeMemoryMappedFileHandle` via a field
  * Option 2.2 "Smuggle" the file stream via the `SafeMemoryMappedFileHandle` unsealing & deriving from `SafeMemoryMappedFileHandle`
  * Option 3: do what's proposed

```C#
namespace System.IO.MemoryMappedFiles
{
    public partial class MemoryMappedFile
    {
        public MemoryMappedFile(SafeMemoryMappedFileHandle handle, IDisposable fileStream);
    }
}
```

## Implement IAsyncDisposable for XmlWriter and XmlReader

**Approved** | [#runtime/33618](https://github.com/dotnet/runtime/issues/33618#issuecomment-610535975) | [Video](https://www.youtube.com/watch?v=POdrqNOlY5g&t=0h38m26s)

* We should not make `XmlReader` implement `IAsyncDisposable`
* We should not have a `CloseAsync`
* We don't want a cancelable `DisposeAsync`, for scenarios that need that we add an overload to `FlushAsync` that accepts a cancellation token. We generally want to dispose resources even when you're cancelled.

```C#
namespace System.Xml
{
    public partial class XmlWriter : IAsyncDisposable
    {
        public ValueTask DisposeAsync();
        protected virtual ValueTask DisposeAsyncCore();
    }
}
```

## Provide opt-in for custom converters to handle null

**Needs Work** | [#runtime/34439](https://github.com/dotnet/runtime/issues/34439#issuecomment-610556104) | [Video](https://www.youtube.com/watch?v=POdrqNOlY5g&t=0h57m13s)

* In this proposal the converter is only called when a `null` value is included in the payload. If the property is missing in the payload, the the convert still won't be called. Does this satisfy the customer request? While this should probably be an independent feature it seems we'll want to make sure the design for both will work well together.
* Should this be on the non-generic base `JsonConverter`?
  * We concluded no, because the `Read`/`Write` methods are also only on `JsonConverter<T>`

```C#
namespace System.Text.Json.Serialization
{
    public partial class JsonConverter<T>
    {
        public virtual bool HandleNull { get; }
    }
}
```

## JsonSerializer support for non-public accessors and fields

**Approved** | [#runtime/34558](https://github.com/dotnet/runtime/issues/34558#issuecomment-610571471) | [Video](https://www.youtube.com/watch?v=POdrqNOlY5g&t=1h39m35s)

* Let's rename `JsonMemberAttribute` to `JsonIncludeAttribute`
* We should change `JsonIgnoreAttribute` to include fields
* We don't see a need for `JsonObjectAttribute` it should be a per property/field decisions
* We should treat read-only fields like get-only properties
* We can decide to support private/internal fields/properties by applying `JsonInclude`, but we need think through implications for AOT (generating the serialzier as part of the type would make this possible tho)

```C#
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public bool IncludeFields { get; set; }
    }
}
namespace System.Text.Json.Serialization
{
    [AttributeUsage(AttributeTargets.Property |
                    Attributes.Field, AllowMultiple = false)]
    public sealed class JsonIncludeAttribute : JsonAttribute
    {
        public JsonIncludeAttribute();
    }
}
```

