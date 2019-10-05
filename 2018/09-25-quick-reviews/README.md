# Quick Reviews 9/25/2018

## Add 'split' support for ReadOnlySpan<char> similar to string

**Approved** | [#corefx/26528](https://github.com/dotnet/corefx/issues/26528#issuecomment-424438136) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=0h-41m-28s)

We don't think we want these APIs:

1. They allocate
2. They aren't as convenient as `String.Split`

If you care about allocations, then you want a different API. And if you don't care about allocations, well, then you can just use `String.Split`.

So what API would we like to see? It could be a struct-based enumerator that allows the consumer to `foreach` the individual spans without allocations. Alternatively (or additionally) we could have a method that allows the consumer to pass in a buffer with the locations, which, for the most part, could be `stackalloc`ated.
## BoundedConcurrentQueue<T>

**Needs Work** | [#corefx/24365](https://github.com/dotnet/corefx/issues/24365) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=0h40m55s)

## Capability APIs for runtime code generation

**Approved** | [#corefx/29258](https://github.com/dotnet/corefx/issues/29258#issuecomment-424442400) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=0h46m33s)

This is related: https://github.com/dotnet/standard/issues/832

I believe this feature needs a more comprehensive design. I'll follow-up.
## Vector<T> should have a ctor that accepts ReadOnlySpan<T>

**Approved** | [#corefx/30968](https://github.com/dotnet/corefx/issues/30968#issuecomment-424448085) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=0h53m49s)

@GrabYourPitchforks also suggested these APIs:

```cs
public struct Vector<T> {
    /* new ctor overload proposal */
    public Vector(ReadOnlySpan<byte> values);
   
    /* new instance method proposals */
    public void CopyTo(Span<byte> destination);
    public bool TryCopyTo(Span<byte> destination);
}
```

This would avoid consumers having to re-interpret case the spans. We don't know what happens when the `Vector<T>` is instantiated with `byte` though.  @GrabYourPitchforks, please take a look and file another item.
## Add IPEndPoint.Parse() & .TryParse()

**Approved** | [#corefx/31291](https://github.com/dotnet/corefx/issues/31291#issuecomment-424450665) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=1h9m40s)

We should probably also expose this for `DnsEndPoint`, but that should be another issue. This one looks good as proposed.
## Make internal directory separator helpers public

**Approved** | [#corefx/31570](https://github.com/dotnet/corefx/issues/31570#issuecomment-424461314) | [Video](https://www.youtube.com/watch?v=nJleHEiiX14&t=1h17m49s)

The `EndsIn*` methods seem problematic. The first ones seems fine but we should add support memory.

```c#
namespace System.IO
{
    public static class Path
    {
        public static string TrimEndingDirectorySeparator(string path);
        public static ReadOnlySpan<char> TrimEndingDirectorySeparator(ReadOnlySpan<char> path);
        public static ReadOnlyMemory<char> TrimEndingDirectorySeparator(ReadOnlyMemory<char> path);
    }
}
```

