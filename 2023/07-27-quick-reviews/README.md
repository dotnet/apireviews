# API Review 07/27/2023

## Add Array.Fill method for multi-dimensional array

**NeedsWork** | [#runtime/47213](https://github.com/dotnet/runtime/issues/47213#issuecomment-1654094986) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=0h0m0s)

The proposed `Array.Fill<T>(Array, T)` is too high of a risk of source breaking changes on patterns like filling a long[] or double[] from an int.

During discussion it was suggested that instead the static methods on Array that take an array as the first parameter should/could be transitioned over to MemoryExtensions (or some other destination) as extension methods, and then having one extension method for `T[]` and another for System.Array will better solve the problem (at least when called from extension syntax).  This approach is probably the better path, but needs some investigation.
## Analyzer: Prefer Task.FromResult over Task.Run(() => value)

**Rejected** | [#runtime/67123](https://github.com/dotnet/runtime/issues/67123#issuecomment-1654124248) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=0h19m14s)

The premise behind this analyzer seems sound (and we came up with a lot of cases that would be useful (and safe) in addition to just literals.  However, we don't think that this occurs often enough in real code to warrant the CPU cost of running the analyzer itself.

If someone wants to convince us that this is actually quite widespread, we can revisit it in the future.
## Consider using ValueTask<T> instead of Task<T>

**Approved** | [#runtime/33804](https://github.com/dotnet/runtime/issues/33804#issuecomment-1654150105) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=0h34m56s)

Generally seems good as proposed.  A few notes from the discussion

* It should follow existing conventions for InternalsVisibleTo with internal members.
* It should probably ignore virtual members in the initial implementation, then add them in later.
* The general rule is "if any caller code would have to change, don't recommend this change"
* It only applies to methods with the `async` keyword.

Category: Performance
Level: Info
## Task.WaitAll missing IEnumerable<Task> overload

**Approved** | [#runtime/34106](https://github.com/dotnet/runtime/issues/34106#issuecomment-1654183126) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=0h52m51s)

We expanded the original proposal by
* Adding a defaulted CancellationToken
* Also including WhenAny


```C#
namespace System.Threading.Tasks
{
    public partial class Task
    {
        public static void WaitAll(IEnumerable<Task> tasks, CancellationToken cancellationToken = default);
        public static int WaitAny(IEnumerable<Task> tasks, CancellationToken cancellationToken = default);
    }
}
```

## Add JsonContent.Create overloads that accept JsonTypeInfo.

**Approved** | [#runtime/51544](https://github.com/dotnet/runtime/issues/51544#issuecomment-1654196553) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=1h5m28s)

Looks good as proposed (modulo typos)

```C#
namespace System.Net.Http.Json
{
    public partial class JsonContent
    {
        public static JsonContent Create<TValue>(TValue? inputValue, JsonTypeInfo<TValue> jsonTypeInfo, MediaTypeHeaderValue? mediaType = null);
        public static JsonContent Create(object? inputValue, JsonTypeInfo jsonTypeInfo, MediaTypeHeaderValue? mediaType = null);
    }
}
```
## RequiresLocationAttribute (supporting ref readonly parameters)

**Approved** | [#runtime/85910](https://github.com/dotnet/runtime/issues/85910#issuecomment-1654204609) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=1h10m52s)

Looks good as proposed.  Marking the issue as blocked, though, since it's intentional that this attribute not be added until .NET 9 (to give the compiler feature one version of prototyping/experimentation/etc that might affect the public attribute).

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Parameter, AllowMultiple = false, Inherited = false)]
    public sealed class RequiresLocationAttribute : Attribute
    {
    }
}
```
## Allow specifying indent size and whitespace character when writing JSON with Utf8JsonWriter

**Approved** | [#runtime/63882](https://github.com/dotnet/runtime/issues/63882#issuecomment-1654364240) | [Video](https://www.youtube.com/watch?v=ULSmH4ZxolA&t=1h14m15s)

After a long discussion, we seem to have returned to using a single string property.

We did update the shape to account for the new source generator attribute, though.

```C#
namespace System.Text.Json
{
    public sealed class JsonSerializerOptions 
    {
        public string IndentText { get; set; }
    }
    
    public struct JsonWriterOptions 
    {
        public string IndentText { get; set; }
    }
}

namespace System.Text.Json.Serialization
{
    public partial class JsonSourceGenerationOptionsAttribute
    {
        public string IndentText { get; set; }
    }
}
```
