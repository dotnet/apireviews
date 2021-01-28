# Quick Reviews 01/28/2021

## Proposal: Vector.Sum(Vector<T>) API for horizontal add

**Approved** | [#runtime/35626](https://github.com/dotnet/runtime/issues/35626#issuecomment-769276218) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Numerics
{
    public static partial class Vector
    {
        public static T Sum<T>(Vector<T> vector) where T : struct;
    }
}
```
## Consider adding SingleToUInt32Bits(float value) to BitConverter

**Approved** | [#runtime/36469](https://github.com/dotnet/runtime/issues/36469#issuecomment-769282102) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h5m1s)

* Looks good as proposed

```C#
namespace System
{
    public static class BitConverter
    {
        public static unsafe ulong DoubleToUInt64Bits(double value);
        public static unsafe double UInt64BitsToDouble(ulong value);

        public static unsafe ushort HalfToUInt16Bits(Half value);
        public static unsafe Half UInt16BitsToHalf(ushort value);

        public static unsafe uint SingleToUInt32Bits(float value);
        public static unsafe float UInt32BitsToSingle(uint value);
    }
}
```
## Add strict validation to System.IO.Compression.GZipStream

**NeedsWork** | [#runtime/47563](https://github.com/dotnet/runtime/issues/47563#issuecomment-769287654) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h14m25s)

@mfkl is there any chance that you could research if other compression methods allow for such validation? In such a case, we should add overloads to other ctors too. Once we get there, we are going to mark the issue as ready for review, go through API Design Review, get the API accepted (with possible modifications of the proposal), and then re-open your PR. 
## Expose RoundUpToPowerOf2 from BitOperations and optimize it

**Approved** | [#runtime/43135](https://github.com/dotnet/runtime/issues/43135#issuecomment-769289340) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h24m54s)

* Looks good as proposed

```C#
namespace System.Numerics
{
    public static class BitOperations
    {
        public static uint RoundUpToPowerOf2(uint i);
        public static ulong RoundUpToPowerOf2(ulong i);
    }
}
```
## Add APIs to make nint & nuint match Int32 and Int64

**Approved** | [#runtime/46467](https://github.com/dotnet/runtime/issues/46467#issuecomment-769292235) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h27m26s)

* Looks good as proposed

```C#
namespace System
{
    public class IntPtr
    {
        public static IntPtr Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
        public static bool TryParse(ReadOnlySpan<char> s, out IntPtr result);
        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out IntPtr result);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default, IFormatProvider? provider = null);
    }
    public partial class UIntPtr
    {
        public static UIntPtr Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
        public static bool TryParse(ReadOnlySpan<char> s, out UIntPtr result);
        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out UIntPtr result);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default, IFormatProvider? provider = null);
     }
}
```
## Developers can get immediate feedback on validation problems

**Approved** | [#runtime/36391](https://github.com/dotnet/runtime/issues/36391#issuecomment-769300115) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h32m59s)

* Looks good as proposed
* `ValidateAnnotationsEagerly` should be `ValidateDataAnnotationsEagerly` to be a suffix of the existing APIs
* Why do we need both `ValidateEagerly` and `ValidateDataAnnotationsEagerly`?
    - We concluded we only need `ValidateEagerly` for now
* Is `Eagerly` the right term here? I think we'd prefer `OnStartup` Any opinions @davidfowl?

```C#
namespace Microsoft.Extensions.DependencyInjection
{
    public static class OptionsBuilderDataAnnotationsExtensions
    {
    //  Already exists:
    //  public static OptionsBuilder<TOptions> ValidateDataAnnotations<TOptions>(this OptionsBuilder<TOptions> optionsBuilder) where TOptions : class;
        public static OptionsBuilder<TOptions> ValidateOnStartup<TOptions>(this OptionsBuilder<TOptions> optionsBuilder) where TOptions : class;
    }
}
```
## Refering to unknown platform names should result in warnings 

**Approved** | [#runtime/45851](https://github.com/dotnet/runtime/issues/45851#issuecomment-769312213) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=0h47m25s)

The severity should be a warning and on by-default. It seems this can't produce false positives, especially with the escape mechanism of adding a net-new platform via the `SupportedPlatform` item group in the project file. /cc @marek-safar 
## [Platform Compatibility Analyzer] Nested APIs shouldn't be allowed to expand the platform set

**Approved** | [#runtime/43971](https://github.com/dotnet/runtime/issues/43971#issuecomment-769319631) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=1h7m17s)

Makes sense. Two thoughts:

1. Should be a warning and on by-default.
2. The analyzer today can only handle an API coming and going out of support. In principle, there is nothing ill-defined about an API being re-supported after it went out of support, but that seems rare. It feels OK to treat this as a warning until we need to support it.
## IEnumerable should have a extension for creating fixed size chunks

**Approved** | [#runtime/27449](https://github.com/dotnet/runtime/issues/27449#issuecomment-769321329) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=1h21m18s)

* Looks good as proposed.
* Note: Every time we add a member to `Enumerable` that returns `IEnumerable<T>` we need to add a corresponding member on `Queryable` that returns `IQueryable<T>` (and we do have a test for that)

```C#
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<T[]> Chunk(this IEnumerable<T> source, int size);
    }
    public static class Queryable
    {
        public static IQueryable<T[]> Chunk(this IQueryable<T> source, int size);
    }
}
```
## Add Enumerable.*By operators (DistinctBy, ExceptBy, IntersectBy, UnionBy, MinBy, MaxBy)

**Approved** | [#runtime/27687](https://github.com/dotnet/runtime/issues/27687#issuecomment-769339626) | [Video](https://www.youtube.com/watch?v=ynpmIT-DlO0&t=1h24m13s)

* Looks good as proposed.
* We should make sure the parameter names

```C#
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TSource> DistinctBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);
        public static IEnumerable<TSource> DistinctBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer);
        
        public static IEnumerable<TSource> ExceptBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector);
        public static IEnumerable<TSource> ExceptBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer);

        public static IEnumerable<TSource> ExceptBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TKey> second, Func<TSource, TKey> keySelectorFirst);
        public static IEnumerable<TSource> ExceptBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TKey> second, Func<TSource, TKey> keySelectorFirst, IEqualityComparer<TKey>? comparer);

        public static IEnumerable<TSource> IntersectBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector);
        public static IEnumerable<TSource> IntersectBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer);

        public static IEnumerable<TSource> IntersectBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TKey> second, Func<TSource, TKey> keySelectorFirst);
        public static IEnumerable<TSource> IntersectBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TKey> second, Func<TSource, TKey> keySelectorFirst, IEqualityComparer<TKey>? comparer);
        
        public static IEnumerable<TSource> UnionBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector);
        public static IEnumerable<TSource> UnionBy<TSource, TKey>(this IEnumerable<TSource> first, IEnumerable<TSource> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer);
        
        public static TSource MinBy<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);
        public static TSource MinBy<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector, IComparer<TResult>? comparer);
        
        public static TSource MaxBy<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);
        public static TSource MaxBy<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector, IComparer<TResult>? comparer);
        
        // Missing min & max overloads accepting custom comparers added for completeness
        public static TResult Min<TSource, TResult>(this IEnumerable<TSource> source, IComparer<TResult>? comparer);
        public static TResult Max<TSource, TResult>(this IEnumerable<TSource> source, IComparer<TResult>? comparer);
    }
    public static class Queryable
    {
        public static IQueryable<TSource> DistinctBy<TSource, TKey>(this IQueryable<TSource> source, Expression<Func<TSource, TKey>> keySelector);
        public static IQueryable<TSource> DistinctBy<TSource, TKey>(this IQueryable<TSource> source, Expression<Func<TSource, TKey>> keySelector, IEqualityComparer<TKey>? comparer);

        public static IQueryable<TSource> ExceptBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector);
        public static IQueryable<TSource> ExceptBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector, IEqualityComparer<TKey>? comparer);

        public static IQueryable<TSource> ExceptBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TKey> source2, Expression<Func<TSource, TKey>> keySelectorFirst);
        public static IQueryable<TSource> ExceptBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TKey> source2, Expression<Func<TSource, TKey>> keySelectorFirst, IEqualityComparer<TKey>? comparer);

        public static IQueryable<TSource> IntersectBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector);
        public static IQueryable<TSource> IntersectBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector, IEqualityComparer<TKey>? comparer);

        public static IQuerable<TSource> IntersectBy<TSource, TKey>(this IQuerable<TSource> source1, IEnumerable<TKey> source2, Expression<Func<TSource, TKey>> keySelectorFirst);
        public static IQuerable<TSource> IntersectBy<TSource, TKey>(this IQuerable<TSource> source1, IEnumerable<TKey> source2, Expression<Func<TSource, TKey>> keySelectorFirst, IEqualityComparer<TKey>? comparer);

        public static IQueryable<TSource> UnionBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector);
        public static IQueryable<TSource> UnionBy<TSource, TKey>(this IQueryable<TSource> source1, IEnumerable<TSource> source2, Expression<Func<TSource, TKey>> keySelector, IEqualityComparer<TKey>? comparer);

        public static TSource MinBy<TSource, TResult>(this IQueryable<TSource> source, Expression<Func<TSource, TResult>> selector);
        public static TSource MinBy<TSource, TResult>(this IQueryable<TSource> source, Expression<Func<TSource, TResult>> selector, IComparer<TResult>? comparer);

        public static TSource MaxBy<TSource, TResult>(this IQueryable<TSource> source, Expression<Func<TSource, TResult>> selector);
        public static TSource MaxBy<TSource, TResult>(this IQueryable<TSource> source, Expression<Func<TSource, TResult>> selector, IComparer<TResult>? comparer);

        // Missing min & max overloads accepting custom comparers added for completeness
        public static TResult Min<TSource, TResult>(this IQueryable<TSource> source, IComparer<TResult>? comparer);
        public static TResult Max<TSource, TResult>(this IQueryable<TSource> source, IComparer<TResult>? comparer);
    }
}
```
