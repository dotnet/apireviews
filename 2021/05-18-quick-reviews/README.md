# Quick Reviews 05/18/2021

## Avoid unnecessary allocations when using FileStream

**Approved** | [#runtime/15088](https://github.com/dotnet/runtime/issues/15088#issuecomment-843381781) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=0h0m0s)

@tannergooding mentioned that there may be a more generalized allocator/management feature in the works, so rather than accepting a buffer now as well as an allocator "soon", we feel that the right answer for now is just to take the buffer size, not a user provided buffer.

```C#
namespace System.IO
{
    partial class FileStreamOptions
    {
       public int BufferSize { get; set; }
    }
}
```

## Consider making TryGetEnumeratedCount an interface method with default implementation

**NeedsWork** | [#runtime/52775](https://github.com/dotnet/runtime/issues/52775#issuecomment-843404922) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=0h22m2s)

We currently have apprehension about adding DIMs at this level.  For this particular method we're concerned that the obvious solution would be to re-implement the methods in `ICollection<T>` and `IReadOnlyCollection<T>`, which now provides multiple non-unified implementations to concrete types; so that would manifest as a runtime breaking change for any type that already implements those two interfaces.

But we agree that we could use some better concrete guidelines here going forward.
## Support safe trimmability for System.Text.Json.Nodes.JsonValue

**Approved** | [#runtime/52773](https://github.com/dotnet/runtime/issues/52773#issuecomment-843424777) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=0h51m9s)

We didn't feel it was worth renaming the linker-unfriendly version.  So now it's just adding the specialized versions.

We discussed the operators, and had some mixed feelings.  Since they're not quite related to the rest of the changes we removed them from here (they can be brought up later as a separate issue).


```diff
namespace System.Text.Json.Nodes
{
  public partial class JsonValue
  {
+  public static JsonValue? Create<T>(T? value, JsonTypeInfo<T> jsonTypeInfo, JsonNodeOptions? options = null)

-  public static JsonValue? Create<T>(T? value, JsonNodeOptions? options = null)
+  public static JsonValue? Create<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicProperties)]T>(T? value, JsonNodeOptions? options = null)

+  public static JsonValue Create(bool value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(byte value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(char value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(System.DateTime value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(System.DateTimeOffset value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(decimal value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(double value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(System.Guid value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(short value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(int value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(long value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(bool? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(byte? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(char? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(System.DateTimeOffset? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(System.DateTime? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(decimal? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(double? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(System.Guid? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(short? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(int? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(long? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue? Create(sbyte? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(float? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(System.Text.Json.JsonElement? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue? Create(ushort? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue? Create(uint? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue? Create(ulong? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue Create(sbyte value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue Create(float value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(string? value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  public static JsonValue? Create(System.Text.Json.JsonElement value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue Create(ushort value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue Create(uint value, JsonNodeOptions? options = default(JsonNodeOptions?));
+  [System.CLSCompliantAttribute(false)]
+  public static JsonValue Create(ulong value, JsonNodeOptions? options = default(JsonNodeOptions?));
}
```

It also looks like `JsonArray.Add<T>` needs the dynamically accessed member attributes.  Adding the appropriate attributes is also approved with this issue.
## Synchronous deserialize and serialize to a stream with JsonSerializer.

**Approved** | [#runtime/1574](https://github.com/dotnet/runtime/issues/1574#issuecomment-843447885) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=1h40m41s)

Adding synchronous method parity sounds good.

```C#
namespace System.Text.Json
{
    partial class JsonSerializer
    {
        // Proposal to add following synchronous APIs:
        public static object Deserialize(Stream utf8Json, 
            Type returnType, 
            JsonSerializerOptions options = null);
        
        public static TValue Deserialize<TValue>(Stream utf8Json,
            JsonSerializerOptions options = null);
    
        public static void Serialize(Stream utf8Json, 
            object value, 
            Type inputType, 
            JsonSerializerOptions options = null);
        
        public static void Serialize<TValue>(Stream utf8Json, 
            TValue value, 
            JsonSerializerOptions options = null);
    }
}
```

Also add the overloads for the new `JsonTypeInfo<T>` versions from the source generator.
## Obsolete SuppressIldasmAttribute

**Approved** | [#runtime/40622](https://github.com/dotnet/runtime/issues/40622#issuecomment-843449308) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=1h45m27s)

Approved as proposed, but use the next available diagnostic ID.


```diff
namespace System.Runtime.CompilerServices
{
+   [Obsolete("SuppressIldasmAttribute has no effect in .NET 6.0+ applications.")]
    [AttributeUsage(AttributeTargets.Assembly | AttributeTargets.Module)]
    public sealed class SuppressIldasmAttribute : Attribute
    {
        public SuppressIldasmAttribute() { }
    }
}
```
## Provide Marshal.Alloc api that accepts alignment argument

**Approved** | [#runtime/33244](https://github.com/dotnet/runtime/issues/33244#issuecomment-843465390) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=1h47m33s)

* There was an argument that Alloc methods on Marshal should return IntPtr.  So we made a new type to allow it to use `void*` and `nuint`.
* We got rid of all of the "Memory" words, since that's part of the type name.
* We aligned with C runtime names (Alloc => Malloc, AllocAligned => AlignedAlloc)
* We added Calloc, because why not?

```C#
namespace System.Runtime.InteropServices
{
    public static class NativeMemory
    {
        public static unsafe void* Calloc(nuint elementCount, nuint elementSize);
        public static unsafe void* Malloc(nuint byteCount);
        public static unsafe void* Realloc(void* ptr, nuint byteCount);
        public static unsafe void Free(void* ptr);

        public static unsafe void* AlignedAlloc(nuint alignment, nuint byteCount);

        // This may not be needed, if all of our runtimes guarantee that AlignedAlloc can be passed straight to Free.
        public static unsafe void AlignedFree(void* ptr);
    }
}
```
## Add StringContent ctor providing tighter control over charset

**NeedsWork** | [#runtime/17036](https://github.com/dotnet/runtime/issues/17036#issuecomment-843487131) | [Video](https://www.youtube.com/watch?v=by1CiPjnA08&t=2h11m50s)

Triage: We would like to know details about the user scenarios to make sure we understand which APIs we should add and why.
It was not clear if the current APIs are unusable, or if there is performance concern.

@DaveSenn the above will block PR for now, sorry ... let's first clarify what we want to add properly. Thanks for understanding.
