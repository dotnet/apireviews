# Quick Reviews 6/12/2020

## HttpContent.ReadAsStream sync API 

**Approved** | [#runtime/37495](https://github.com/dotnet/runtime/issues/37495) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h0m0s)

## Update UnmanagedCallersOnlyAttribute to align with C# function pointers

**Approved** | [#runtime/37612](https://github.com/dotnet/runtime/issues/37612) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h7m15s)

## TPL Dataflow TransformManyBlock support for IAsyncEnumerable

**Approved** | [#runtime/30863](https://github.com/dotnet/runtime/issues/30863) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h22m21s)

## Consider introducing BitOperations.IsPow2 or Math.IsPow2

**Approved** | [#runtime/31297](https://github.com/dotnet/runtime/issues/31297#issuecomment-643407800) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h30m46s)

Looks good as proposed

```C#
namespace System.Numerics
{
    public static class BitOperations
    {
        public static bool IsPow2(int value);
        public static bool IsPow2(uint value);
        public static bool IsPow2(long value);
        public static bool IsPow2(ulong value);
    }
}
```
## Add API to get bit length of BigInteger

**Approved** | [#runtime/31308](https://github.com/dotnet/runtime/issues/31308#issuecomment-643406480) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h35m57s)

* Makes sense as proposed
* We considered `GetBitCount()` to be consistent with `GetByteLength()` but it seems confusing because it sounds like `GetPopCount()`.

```C#
namespace System.Numerics
{
    public struct BigInteger
    {
        public int GetBitLength();
    }
}
```
## Add [CallerMustBeUnsafe] attribute to denote APIs which should be called in an unsafe block

**Needs Work** | [#runtime/31354](https://github.com/dotnet/runtime/issues/31354#issuecomment-643417759) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=0h46m49s)

* The idea is interesting, but we have concerns:
    - Which kind of APIs would we apply this too? Every p/Invoke? `CollectionMarshal.GetSpanForList()`?
    - The name should reflect what the method does, rather than what mechanically has to happen on the call side.
    - Would we turn this on for existing APIs? If so, it seems this analyzer needs to opt-in. If it's opt-in, is it still useful?


```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Field |
                    AttributeTargets.Method |
                    AttributeTargets.Property,
                    AllowMultiple = false,
                    Inherited = true)]
    public sealed class CallerMustBeUnsafeAttribute : Attribute
    {
    }
}
```
## SequenceReader.TryReadTo(out ReadOnlySpan<T> sequence, ReadOnlySpan<T> delimiter)

**Rejected** | [#runtime/31355](https://github.com/dotnet/runtime/issues/31355#issuecomment-643421064) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=1h11m27s)

* We have an existing API that returns the sequence.
* We don't want to offer an API that might allocate unexpectedly, especially on a type that is about avoiding allocations.

```C#
namespace System.Buffers
{
    public ref struct SequenceReader<T>
    {
        // Existing API
        // public bool TryReadTo(out ReadOnlySequence<T> sequence, T delimiter, bool advancePastDelimiter = true);

        public bool TryReadTo(out ReadOnlySpan<T> span, ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
    }
}
```
## Utf8Parser overloads for ReadOnlySequence<byte>

**Needs Work** | [#runtime/31357](https://github.com/dotnet/runtime/issues/31357#issuecomment-643427770) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=1h19m33s)

@davidfowl have tried using APIs like this in Kestrel or is this API proposal a hypothetical? The question is whether you can the perf, because parsers should really be over spans, not sequences.  We already said we won't do parsers of `Memory<T>` because accessing the span is not free. Doing it over sequences would be even worse.
## ChannelReader<T>.TryPeek

**Approved** | [#runtime/31362](https://github.com/dotnet/runtime/issues/31362#issuecomment-643438552) | [Video](https://www.youtube.com/watch?v=4sksWrrgXJg&t=1h35m55s)

* We should add a virtual `CanPeek` that returns `false`
* We'll want more feedback so we should punt this to .NET 6.

```C#
namespace System.Threading.Channels
{
    public abstract partial class ChannelReader<T>
    {
        public virtual bool CanPeek => false;

        public virtual bool TryPeek([MaybeNullWhen(false)] out T item)
        {
            item = default!;
            return false;
        }
    }
}
```
