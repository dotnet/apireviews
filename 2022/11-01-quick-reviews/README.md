# API Review 11/01/2022

## Adding GpioPin to GpioController in .NET IoT

**Approved** | [#iot/1920](https://github.com/dotnet/iot/issues/1920#issuecomment-1298921291)

* The binary breaking change seems to be mitigated by a fairly tight coupling to higher level types that ship out of the same repository, and low usage of these primitive types directly.
* Based on discussion it seems like the controller needs a Toggle method as well  (or TogglePin, whatever you think is best)
* During discussion it was stated that GpioPin was going to reach through straight to the driver, potentially bypassing any overridden methods in the controller class.  This may be too much of a surprise.
  * It was mentioned that the straight-to-the-driver behavior could be conditional on whether this.GetType() == typeof(GpioController).

```diff
namespace System.Device.Gpio;

public class GpioController : IDisposable
{
// Changing current OpenPin overloads to return a GpioPin instead of void. Binary breaking, non source breaking.
-   public void OpenPin(int pinNumber) { }
+   public GpioPin OpenPin(int pinNumber) { }

-   public void OpenPin(int pinNumber, PinMode mode) { }
+   public GpioPin OpenPin(int pinNumber, PinMode mode) { }

-   public void OpenPin(int pinNumber, PinMode mode, PinValue initialValue) { }
+   public GpioPin OpenPin(int pinNumber, PinMode mode, PinValue initialValue) { }

+   public void Toggle(int pinNumber) { }
}

+public sealed class GpioPin
+{
+    public int PinNumber { get; }
+    public PinMode GetPinMode() { }
+    public bool IsPinModeSupported(PinMode pinMode) { }
+    public void SetPinMode(PinMode value) { }
+    public PinValue Read() { }
+    public void Write(PinValue value) { }
+    public event PinChangeEventHandler ValueChanged;
+    public void Toggle() { }
+}
```
## Provide optimized read-only collections

**Approved** | [#runtime/67209](https://github.com/dotnet/runtime/issues/67209#issuecomment-1299003340)

* Based on discussion the abstracts were removed from everything except the type.
* We split the extension methods into types that were the output type, but non-generic, to facilitate static calls.
* We decided a new namespace (System.Collections.Frozen) was better than Immutable.
* We copied in the Enumerable.ToDictionary shapes as overloads of ToFrozenDictionary
* We added TryGetValue to FrozenSet to match this recent addition to HashSet.
* There was a question of should FrozenSet's Contains take the parameter as `in`, and the conclusion was no.

```C#
namespace System.Collections.Frozen
{
    public static class FrozenDictionary
    {
        public static FrozenDictionary<TKey, TValue> ToFrozenDictionary<TKey, TValue>(
            this IEnumerable<KeyValuePair<TKey, TValue>> source, IEqualityComparer<TKey>? comparer = null)
            where TKey : notnull;

        public static FrozenDictionary<TKey, TSource> ToFrozenDictionary<TSource, TKey>(
            this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null)
            where TKey : notnull;

        public static FrozenDictionary<TKey, TElement> ToFrozenDictionary<TSource, TKey, TElement>(
            this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey>? comparer = null)
            where TKey : notnull;
    }

    public static class FrozenSet
    {
        public static FrozenSet<T> ToFrozenSet<T>(
            this IEnumerable<T> source, IEqualityComparer<T>? comparer = null);
    }

    public abstract class FrozenDictionary<TKey, TValue> :
        IDictionary<TKey, TValue>, IReadOnlyDictionary<TKey, TValue>, IDictionary
        where TKey : notnull
    {
        internal FrozenDictionary(); // not designed for external extensibility

        public static FrozenDictionary<TKey, TValue> Empty { get; }

        public IEqualityComparer<TKey> Comparer { get; }
        public int Count { get; }

        public ImmutableArray<TKey> Keys { get; }
        public ImmutableArray<TValue> Values { get; }

        public bool ContainsKey(TKey key);
        public bool TryGetValue(TKey key, [MaybeNullWhen(false)] out TValue value);

        public ref readonly TValue this[TKey key] { get; }
        public ref readonly TValue GetValueRefOrNullRef(TKey key);

        public void CopyTo(KeyValuePair<TKey, TValue>[] destination, int destinationIndex);
        public void CopyTo(Span<KeyValuePair<TKey, TValue>> destination);

        public Enumerator GetEnumerator();
        public struct Enumerator : IEnumerator<KeyValuePair<TKey, TValue>>
        {
            public readonly KeyValuePair<TKey, TValue> Current { get; }
            public bool MoveNext();
        }
    }

    public abstract class FrozenSet<T> : ISet<T>, ICollection,
#if NET5_0_OR_GREATER
        IReadOnlySet<T>
#else
        IReadOnlyCollection<T>
#endif
    {
        internal FrozenSet(); // not designed for external extensibility

        public static FrozenSet<T> Empty { get; }

        public IEqualityComparer<T> Comparer { get; }
        public int Count { get; }

        public ImmutableArray<T> Items { get; }

        public bool Contains(T item);
        public bool TryGetValue(T equalValue, out T actualValue);

        public void CopyTo(Span<T> destination);
        public void CopyTo(T[] destination, int destinationIndex);

        public bool IsProperSubsetOf(IEnumerable<T> other);
        public bool IsProperSupersetOf(IEnumerable<T> other);
        public bool IsSubsetOf(IEnumerable<T> other);
        public bool IsSupersetOf(IEnumerable<T> other);
        public bool Overlaps(IEnumerable<T> other);
        public bool SetEquals(IEnumerable<T> other);

        public Enumerator GetEnumerator();
        public struct Enumerator : IEnumerator<T>
        {
            public readonly T Current { get; }
            public bool MoveNext();
        }
    }
}
```
