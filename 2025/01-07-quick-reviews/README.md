# API Review 01/07/2025

## Proposal: Create OOB package for Rune, System.Text.Unicode.Utf8, and related APIs

**Approved** | [#runtime/52947](https://github.com/dotnet/runtime/issues/52947#issuecomment-2576003992) | [Video](https://www.youtube.com/watch?v=SWoWOrwBmvM&t=0h0m0s)

* All that seems to be needed at this time is System.Text.Unicode.Utf8, so that's all we should add
* Instead of a new package, it seems like this can be shoehorned into Microsoft.Bcl.Memory
* The type should forward on net8/net9/net10

```c#
namespace System.Text.Unicode
{
    public static partial class Utf8
    {
        public static OperationStatus FromUtf16(ReadOnlySpan<char> source, Span<byte> destination, out int charsRead, out int bytesWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true) { throw null; }
        public static OperationStatus ToUtf16(ReadOnlySpan<byte> source, Span<char> destination, out int bytesRead, out int charsWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true) { throw null; }
        public static bool IsValid(ReadOnlySpan<byte> value);
    }
}
```
## API for removing and replacing the standard resilience/hedging handlers

**Approved** | [#extensions/5695](https://github.com/dotnet/extensions/issues/5695#issuecomment-2576077281) | [Video](https://www.youtube.com/watch?v=SWoWOrwBmvM&t=0h48m49s)

* The need for AddOrReplace seems niche.  Since there's a workaround, we think that there's not enough evidence at this time to justify adding API for it, or changing the behavior of the Add methods.
* But RemoveAll is useful and common enough.

```c#
namespace Microsoft.Extensions.DependencyInjection;

public static partial class ResilienceHttpClientBuilderExtensions
{
    public static IHttpClientBuilder RemoveAllResilienceHandlers(this IHttpClientBuilder builder);
}
```
## Add readonly `Capacity` property to `PriorityQueue<TElement, TPriority>`

**Approved** | [#runtime/110728](https://github.com/dotnet/runtime/issues/110728#issuecomment-2576082087) | [Video](https://www.youtube.com/watch?v=SWoWOrwBmvM&t=1h31m14s)

* Looks good as proposed.
* Setting (or, rather, influencing) the capacity is already possible with EnsureCapacity and TrimExcess.

```C#
public partial class PriorityQueue<TElement, TPriority>
{
    public int Capacity { get; }
}
```
## ConditionalWeakTable<TKey,TValue>.GetOrAdd

**Approved** | [#runtime/89002](https://github.com/dotnet/runtime/issues/89002#issuecomment-2576126163) | [Video](https://www.youtube.com/watch?v=SWoWOrwBmvM&t=1h34m5s)

* Rather than creating a new delegate type with different arity, we should just use Func.
* When deciding between making the (TKey, TValue) method GetOrAdd or GetValue, we decided to hide the existing members and move everything to the GetOrAdd name, for better alignment with other types with the same semantics.
* We're not obsoleting the old members at this time, just hiding them.

```diff
namespace System.Runtime.CompilerServices;

public sealed partial class ConditionalWeakTable<TKey, TValue>
{
+   public TValue GetOrAdd(TKey key, TValue value);
+   public TValue GetOrAdd(TKey key, Func<TKey, TValue> valueFactory);
+   public TValue GetOrAdd<TArg>(TKey key, Func<TKey, TArg, TValue> valueFactory, TArg factoryArgument)
+       where TArg : allows ref struct;
    
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public TValue GetValue(TKey key, CreateValueCallback createValueCallback);
  
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public TValue GetOrCreateValue(TKey key);

+   [EditorBrowsable(EditorBrowsableState.Never)]
    public delegate TValue CreateValueCallback(TKey key);
}
```
## GCHandle<T> (like GCHandle, but this time it's great™️)

**Approved** | [#runtime/94134](https://github.com/dotnet/runtime/issues/94134#issuecomment-2576202334) | [Video](https://www.youtube.com/watch?v=SWoWOrwBmvM&t=2h1m25s)

* Rather than taking the GCHandleType parameter in a constructor we separated out the WeakGCHandle concept to a 3rd type.
* Consider declaring and implementing `IEquatable<self>`
* We went with TryGetTarget/SetTarget on WeakGCHandle to mirror `WeakReference<T>`
* We changed AddrOf to GetAddressOf as a prefix.

```c#
namespace System.Runtime.InteropServices;

public struct GCHandle<T> : System.IDisposable
    where T : class?
{
    public GCHandle(T value);

    public readonly bool IsAllocated { get; }
    public readonly T Target { get; set; }

    public static GCHandle<T> FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(GCHandle<T> value);

    public void Dispose();
}

public struct PinnedGCHandle<T> : System.IDisposable
    where T : class?
{
    public PinnedGCHandle(T value);

    public readonly bool IsAllocated { get; }
    public readonly T Target { get; set; }
    public readonly void* GetAddressOfObjectData();

    public static PinnedGCHandle<T> FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(PinnedGCHandle<T> value);

    public void Dispose();
}

public struct WeakGCHandle<T> : System.IDisposable
    where T : class?
{
    public WeakGCHandle(T value, bool trackResurrection = false);

    public readonly bool IsAllocated { get; }

    public readonly bool TryGetTarget([NotNullWhen(true)] out T? target);
    public readonly void SetTarget(T target);

    public static WeakGCHandle<T> FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(WeakGCHandle<T> value);

    public void Dispose();
}

public static class GCHandleExtensions
{
    public static unsafe T* GetAddressOfArrayData<T>(this PinnedGCHandle<T[]?> handle);
    public static unsafe char* GetAddressOfStringData(this PinnedGCHandle<string?> handle);
}
```


