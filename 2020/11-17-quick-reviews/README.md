# Quick Reviews 11/17/2020

## Add System.Collections.Generic.PriorityQueue<TElement, TPriority>

**Approved** | [#runtime/43957](https://github.com/dotnet/runtime/issues/43957#issuecomment-729157090) | [Video](https://www.youtube.com/watch?v=bvoCR1hXMfw&t=0h0m0s)

* Add `EnqueueRange(IEnumerable<TElement> values, TPriority priority)`
* Implementing `IEnumerable` might be problematic because consumers are likely expecting it to return the elements in the order in which `Dequeue` would (or at least priority order)
    - That argument likely also applies to `Elements`, so we should removed that
    - Instead add a `public UnorderedItemsCollection UnorderedItems { get; }` (and should not allocate on each get)
        - It would not be snapshot, you can enumerate while modifying, just like any other collection
* Consider adding `Contains`
* This collection will be shipped in the same assembly that contains the other types in `System.Collections.Generic`

```C#
namespace System.Collections.Generic
{
    public class PriorityQueue<TElement, TPriority>
    {
        public PriorityQueue();
        public PriorityQueue(int initialCapacity);
        public PriorityQueue(IComparer<TPriority>? comparer);
        public PriorityQueue(int initialCapacity, IComparer<TPriority>? comparer);
        public PriorityQueue(IEnumerable<(TElement Element, TPriority Priority)> values);
        public PriorityQueue(IEnumerable<(TElement Element, TPriority Priority)> values, IComparer<TPriority>? comparer);
        public int Count { get; }
        public IComparer<TPriority> Comparer { get; }
        public void Enqueue(TElement element, TPriority priority);
        public TElement Peek();
        public TElement Dequeue();
        public bool TryDequeue([MaybeNullWhen(false)] out TElement element, [MaybeNullWhen(false)] out TPriority priority);
        public bool TryPeek([MaybeNullWhen(false)] out TElement element, [MaybeNullWhen(false)] out TPriority priority);
        public TElement EnqueueDequeue(TElement element, TPriority priority);
        public void EnqueueRange(IEnumerable<(TElement Element, TPriority Priority)> values);
        public void Clear();
        public void EnsureCapacity(int capacity);
        public void TrimExcess();
        public UnorderedItemsCollection UnorderedItems { get; }
        public class UnorderedItemsCollection : IReadOnlyCollection<(TElement, TPriority)>, ICollection
        {
            public struct Enumerator : IEnumerator<(TElement, TPriority)>, IEnumerator { }
            public Enumerator GetEnumerator();
        }
    }
}
```
## Add System.Math.SinCos(..) or teach the JITter to calculate Sin and Cos together.

**Approved** | [#runtime/43169](https://github.com/dotnet/runtime/issues/43169#issuecomment-729162076) | [Video](https://www.youtube.com/watch?v=bvoCR1hXMfw&t=1h26m59s)

* Seems the register allocator will optimize the tuple the best and that also seems to make the most sense, API wise:

```C#
namespace System
{
    public static partial class Math
    {
        public (double Sin, double Cos) SinCos(double x);
    }
    public static partial class MathF
    {
        public (float Sin, float Cos) SinCos(float x);
    }
}
```

## Add System.Math API overloads for nint/nuint

**Approved** | [#runtime/43733](https://github.com/dotnet/runtime/issues/43733#issuecomment-729164596) | [Video](https://www.youtube.com/watch?v=bvoCR1hXMfw&t=1h35m32s)

Looks good as proposed

```C#
namespace System
{
    public static class Math
    {
        public static nint Abs(nint value);
        public static nuint Clamp(nuint value, nuint min, nuint max);
        public static nint Clamp(nint value, nint min, nint max);
        public static nint DivRem(nint a, nint b, out nint result);
        public static nint Max(nint val1, nint val2);
        public static nuint Max(nuint val1, nuint val2);
        public static nint Min(nint val1, nint val2);
        public static nuint Min(nuint val1, nuint val2);
        public static nint Sign(nint value);
    }
}
```

## Add System.Math.DivRem for ulong and other integers

**Approved** | [#runtime/42156](https://github.com/dotnet/runtime/issues/42156#issuecomment-729167820) | [Video](https://www.youtube.com/watch?v=bvoCR1hXMfw&t=1h40m6s)

* Let's move to the value tuple pattern
* Let's only addd the tuple pattern for the new overloads
* But let's also add value pattern overloads for the existing types

```C#
namespace System
{
    public static partial class Math
    {
        public static (byte Quotient, byte Remainder) DivRem(byte left, byte right);
        public static (sbyte Quotient, sbyte Remainder) DivRem(sbyte left, sbyte right);
        public static (short Quotient, short Remainder) DivRem(short left, short right);
        public static (ushort Quotient, ushort Remainder) DivRem(ushort left, ushort right);
        public static (int Quotient, int Remainder) DivRem(int left, int right);
        public static (uint Quotient, uint Remainder) DivRem(uint left, uint right);
        public static (long Quotient, long Remainder) DivRem(long left, long right);
        public static (ulong Quotient, ulong Remainder) DivRem(ulong left, ulong right);
        public static (nint Quotient, nint Remainder) DivRem(nint left, nint right);
        public static (nuint Quotient, nuint Remainder) DivRem(nuint left, nuint right);
     }
}
```
