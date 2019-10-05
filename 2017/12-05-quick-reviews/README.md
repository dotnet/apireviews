# Quick Reviews 12/5/2017

## Regex should have a static TryParse() method

**Rejected** | [#corefx/25303](https://github.com/dotnet/corefx/issues/25303#issuecomment-349394486) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=0h-6m-16s)

We don't believe this API is very useful, because

1. `TryParse` is really about parsing user input in tight loops, which regex shouldn't be.
2. Returning a `bool` isn't very informative. Granted, our current `ArgumentException` we expose information as a string, which is not machine readable, but that's probably the area we should be improving instead.
3. Plumbing it through the current engine would be non-trival as we throw `ArgumentException` left and right.

We already have plans to rewrite the regex engine; when we do, we should think about diagnostics.
## Allow for ArrayPool<T> to create buffers with a different minimum length

**Needs Work** | [#corefx/12644](https://github.com/dotnet/corefx/issues/12644#issuecomment-349398101) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=0h15m44s)

The ordering is unfortunate. @KrzysztofCwalina mentioned that we might want to take more options in the future to customize how the sizes are spread inside the pool; this indicates that we probably shouldn't take more values as individual arguments but instead introduce a new class that holds the the options like this:

```csharp
public class ArrayPoolOptions
{
    public int MaxArrayLength { get; set; }
    public int MaxArraysPerBucket { get; set; }
    public int MinArrayLength { get; set; }
}

public static class ArrayPool<T>
{
    public static ArrayPool<T> Create();
    public static ArrayPool<T> Create(int maxArrayLength, int maxArraysPerBucket);
    public static ArrayPool<T> Create(ArrayPoolOptions options);
}
```

(this needs more design, but illustrates the point)
## Add Dictionary<TKey, TValue>.Capacity to let you resize map after created

**Approved** | [#corefx/24445](https://github.com/dotnet/corefx/issues/24445#issuecomment-349406685) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=0h28m22s)

Looks like the API needs more discussion.
## ReadOnlySpan<T>.DangerousGetPinnableReference should return a readonly reference

**Needs Work** | [#corefx/23881](https://github.com/dotnet/corefx/issues/23881#issuecomment-349408257) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=0h56m21s)

We don't care super much; we see the value in promoting generally safe behavior so the API suggestion looks sensible to me. However, we lack expertise to judge whether this causes clutter (additional casting, like C++ const). So I'd leave it to runtime/compiler folks to decided what to do here.
## Change OwnedMemory Pin to take an optional integer offset

**Approved** | [#corefx/25229](https://github.com/dotnet/corefx/issues/25229) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h2m30s)

## Span extension method to slice off of a string instance

**Approved** | [#corefx/24072](https://github.com/dotnet/corefx/issues/24072#issuecomment-349412729) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h4m43s)

That seems like a good suggestion, but we should do the same for arrays.
## MemoryExtensions.SequenceCompare

**Approved** | [#corefx/16878](https://github.com/dotnet/corefx/issues/16878#issuecomment-349414287) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h17m30s)

We should change the name to `SequenceCompareTo` (notice the `To` at the end).

```csharp
class static SpanExtensions
{
    static int SequenceCompareTo(this ReadOnlySpan<byte> first, ReadOnlySpan<byte> second);
}
```
## String should implement IReadOnlyList<char>

**Rejected** | [#corefx/14336](https://github.com/dotnet/corefx/issues/14336#issuecomment-349415239) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h19m59s)

String isn't (logically) a collection of chars, so giving a read-only view seems off. Considering how far we're with the new span based APIs, I'm not convinced this interface is adding a ton of value for code that cares about performance (and code that doesn't has alternatives today).
## Add SpanExtensions.AsVector

**Approved** | [#corefx/24343](https://github.com/dotnet/corefx/issues/24343#issuecomment-349418624) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h26m48s)

We should align it with the existing APIs, so I'd rather make this a ctor on `Vector<T>` itself:

```csharp
namespace System.Numerics
{
    public struct Vector<T>
    {
        // Existing API
        public Vector(T[] values);
        // Proposed API
        public Vector(Span<T> values);
    }
}
```
## Expose RuntimeWrappedException constructor

**Approved** | [#corefx/24946](https://github.com/dotnet/corefx/issues/24946) | [Video](https://www.youtube.com/watch?v=BI3iXFT8H7E&t=1h38m8s)

