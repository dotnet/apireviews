# Quick Reviews 1/19/2018

## Change OwnedMemory<T> to IOwnedMemory<T>

**Rejected** | [#corefx/26002](https://github.com/dotnet/corefx/issues/26002#issuecomment-359105642) | [Video](https://www.youtube.com/watch?v=Qcqn-yv1X0o&t=-1h0m-9s)

We decided against it. We cannot model read-only spans with this interface unless we're creating RW and RO interfaces for this.
## Move Span.DangerousCreate to MemoryMarshal.Create

**Approved** | [#corefx/26139](https://github.com/dotnet/corefx/issues/26139#issuecomment-359111438) | [Video](https://www.youtube.com/watch?v=Qcqn-yv1X0o&t=0h13m35s)

We should consider changing the second overload to be `in`. Otherwise, it looks good as proposed.
## Move Span.NonPortableCast to MemoryMarshal and rename to Cast

**Approved** | [#corefx/26368](https://github.com/dotnet/corefx/issues/26368#issuecomment-359109249) | [Video](https://www.youtube.com/watch?v=Qcqn-yv1X0o&t=0h38m14s)

Looks good as proposed. We can add a more specialized API that handles the `else` case if we we want to. Only difference from the proposal is to leave it on `MemoryExtensions` as the API is now safe.


```csharp
namespace System
{
    public static partial class MemoryExtensions
    {
        public static bool TryCast<TFrom, TTo>(this ReadOnlySpan<TFrom> source, out ReadOnlySpan<TTo> output) where TFrom : struct where TTo : struct;
        public static bool TryCast<TFrom, TTo>(this Span<TFrom> source, out Span<TTo> output) where TFrom : struct where TTo : struct;
    }
}
```

We'll also leave the `NonPortableCast` API but we'll move it `MemoryMarshal`. We need to make sure it's not an extension method, under which case we can rename it to just `Cast`.
