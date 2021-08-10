# API Review 08/10/2021

## Provide a way to create types from the MetadataLoadContext

**NeedsWork** | [#runtime/56914](https://github.com/dotnet/runtime/issues/56914)

## Add Metrics overloads taking more tags

**NeedsWork** | [#runtime/56936](https://github.com/dotnet/runtime/issues/56936#issuecomment-896190227)

Based on the size of the parameters and the cost of passing all of those KeyValuePairs, we feel that an approach similar to


```C#
counter.Add(
    10,
    new TagSet
    {
        { "K1", "V1" },
        { "K2", "V2" },
        { "K3", "V3" },
        { "K4", "V4" },
        { "K5", "V5" },
        { "K6", "V6" },
        { "K7", "V7" },
        { "K8", "V8" },
    });
);

namespace System.Diagnostics.Metrics
{
    public struct TagList : IList<KeyValuePair<string, object?>>, IReadOnlyList<KVP<>>
    {
        private KVP<> _kvp1;
        private KVP<> ...
        private KVP<> _kvp8;
        private List<KVP<>>? _spill;

        public TagList(ReadOnlySpan<KVP<>>);
        // other ctors as appropriate
    
        public void Add(string key, object? value);
        public void Add(KeyValuePair<>);

        // optional bulk adders
        //public void AddRange(ReadOnlySpan<KVP<>>);
        //public void AddRange(IEnumerable<KVP<>>);

        // struct enumerator goes here.
        public CopyTo(Span<KVP<>>)

        // indexer, etc.
    }

    partial class Counter<T>
    {
        public void Add(T value, in TagList tags);
    }
```

will give better results and future expansion as needed.
## Allow custom converters for dictionary keys

**Approved** | [#runtime/46520](https://github.com/dotnet/runtime/issues/46520#issuecomment-896196155)


* "WriteTo" felt weird, so changed to "WriteAs".
* "ReadFrom" got changed to "ReadAs" for symmetry
* Nullability of the Ts was stated as not necessarily accurate.  Fix as appropriate.

```C#
namespace System.Text.Json.Serialization
{
    partial class JsonConverter<T>
    {
        protected virtual T? ReadAsPropertyName(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options) => throw null;
        protected virtual void WriteAsPropertyName(Utf8JsonWriter writer, T value, JsonSerializerOptions options) { }
    }
}
```
## [MemoryCache] Add possibility to disable linked cache entries

**Approved** | [#runtime/45592](https://github.com/dotnet/runtime/issues/45592#issuecomment-896214586)


Since this is proposing a breaking change (making the default be non-tracked), going with one centralized option seems the most sensible.
Adding an overload of `MemoryCache.CreateEntry(object key, bool linked)` (or something like that) can be done in the future if entry-level support feels warranted.


```C#
namespace Microsoft.Extensions.Caching.Memory
{
    partial class MemoryCacheOptions
    {
        public bool TrackLinkedCacheEntries { get ; set; } = false;
    }
}
```
## Provide an API to get the native module handle of the native process entry-point module

**Approved** | [#runtime/56331](https://github.com/dotnet/runtime/issues/56331#issuecomment-896219066)

Looks good as proposed.

```C#
namespace System.Runtime.InteropServices
{
     public static class NativeLibrary
     {
          public static IntPtr GetEntryPointModuleHandle();
     }
}
```
## ShiftLeft, ShiftRight for Vector<T>

**Approved** | [#runtime/20510](https://github.com/dotnet/runtime/issues/20510#issuecomment-896224123)


Looks good as proposed, but we normalized to the current parameter names (value and shiftCount, respectively).



```C#
namespace System.Numerics
{
    public static class Vector
    {
        public static Vector<byte> ShiftLeft(Vector<byte> value, int shiftCount);
        public static Vector<short> ShiftLeft(Vector<short> value, int shiftCount);
        public static Vector<int> ShiftLeft(Vector<int> value, int shiftCount);
        public static Vector<long> ShiftLeft(Vector<long> value, int shiftCount);
        public static Vector<nint> ShiftLeft(Vector<nint> value, int shiftCount);
        public static Vector<sbyte> ShiftLeft(Vector<sbyte> value, int shiftCount);
        public static Vector<ushort> ShiftLeft(Vector<ushort> value, int shiftCount);
        public static Vector<uint> ShiftLeft(Vector<uint> value, int shiftCount);
        public static Vector<ulong> ShiftLeft(Vector<ulong> value, int shiftCount);
        public static Vector<nuint> ShiftLeft(Vector<nuint> value, int shiftCount);

        public static Vector<byte> ShiftRightLogical(Vector<byte> value, int shiftCount);
        public static Vector<short> ShiftRightLogical(Vector<short> value, int shiftCount);
        public static Vector<int> ShiftRightLogical(Vector<int> value, int shiftCount);
        public static Vector<long> ShiftRightLogical(Vector<long> value, int shiftCount);
        public static Vector<nint> ShiftRightLogical(Vector<nint> value, int shiftCount);
        public static Vector<sbyte> ShiftRightLogical(Vector<sbyte> value, int shiftCount);
        public static Vector<ushort> ShiftRightLogical(Vector<ushort> value, int shiftCount);
        public static Vector<uint> ShiftRightLogical(Vector<uint> value, int shiftCount);
        public static Vector<ulong> ShiftRightLogical(Vector<ulong> value, int shiftCount);
        public static Vector<nuint> ShiftRightLogical(Vector<nuint> value, int shiftCount);

        public static Vector<short> ShiftRightArithmetic(Vector<short> value, int shiftCount);
        public static Vector<int> ShiftRightArithmetic(Vector<int> value, int shiftCount);
        public static Vector<long> ShiftRightArithmetic(Vector<long> value, int shiftCount);
        public static Vector<nint> ShiftRightArithmetic(Vector<nint> value, int shiftCount);
        public static Vector<sbyte> ShiftRightArithmetic(Vector<sbyte> value, int shiftCount);
    }
}
```
## UnreachableException

**Approved** | [#runtime/35324](https://github.com/dotnet/runtime/issues/35324#issuecomment-896240974)


* This exception made us muse for a long time on wishing that we had better language support for showing that control won't come back. (General ThrowHelper problem)
* We didn't add a static Throw method yet, because of the ThrowHelper control flow problem
* We might want to get the runtime to consider it uncatchable, like StackOverflowException (or what @bartonjs remembers ThreadAbortException doing: you can catch it to log it, but it's being propagated unconditionally at the end of the catch block).
* Since the type is just a .NET N type (not netstandard2.0) the serialization ctor and attr aren't needed
* We discussed UnreachableException vs UnreachableCodeException.  We were split 50/50, so let the tie break go to the proposer
* We're approving it as deriving from SystemException on the assumption we'll give it special treatment.  If it's "just another exception" at the end of the day we should derive directly from Exception.

```C#
#nullable enable
using System.Runtime.Serialization;

namespace System
{
    public sealed class UnreachableException : SystemException
    {
        public UnreachableException()
            : base("The program executed an instruction that was thought to be unreachable.")
        {
        }

        public UnreachableException(string? message)
            : base(message)
        {
        }

        public UnreachableException(string? message, Exception? innerException)
            : base(message, innerException)
        {
        }
    }
}
```
