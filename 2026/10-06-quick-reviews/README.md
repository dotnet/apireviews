# API Review 10/06/2026

## Introduce a fourth type of LazyThreadSafetyMode: ThreadSafeValueOnly

**Approved** | [#runtime/27421](https://github.com/dotnet/runtime/issues/27421#issuecomment-6021921640) | [Video](https://www.youtube.com/watch?v=g3vUhrs6yP4&t=0h0m0s)


* The default of the exception caching depends on the mode already, so there's not a bias for a default-true or default-false position.
* The documentation refers to "exception caching", so "cacheExceptions" seems good.
* Exposing the value-falted state seems like related goodness.
* And we remembered it has an arity-2 derived type.

```csharp
namespace System;

public partial class Lazy<T>
{
    public Lazy(Func<T> valueFactory, LazyThreadSafetyMode mode, bool cacheExceptions);
    public bool IsValueFaulted { get; }
}

public partial class Lazy<T, TMetadata> : Lazy<T>
{
    public Lazy(Func<T> valueFactory, TMetadata metadata, LazyThreadSafetyMode mode, bool cacheExceptions);
}
```
## ReadOnlySpan<char> Replace extensions

**Approved** | [#runtime/29758](https://github.com/dotnet/runtime/issues/29758#issuecomment-6022613829) | [Video](https://www.youtube.com/watch?v=g3vUhrs6yP4&t=0h35m6s)

* OperationStatus makes for a better experience with the destination-too-small
* We've added an out replacementCount because sometimes people want it
* Once we're OperationStatus, we added support for isFinalBlock:false
  * Which is the only case where NeedsMoreData can be returned.
* Let's be conservative with source/destination overlapping, don't try to emulate memmove's massive flexibility.
* UTF-8 can be a later proposal

```csharp
namespace System;

public static partial class MemoryExtensions
{
    public static OperationStatus Replace<T>(
        this ReadOnlySpan<T> source,
        Span<T> destination,
        ReadOnlySpan<T> oldValue,
        ReadOnlySpan<T> newValue,
        out int valuesConsumed,
        out int valuesWritten,
        out int replacementCount,
        IEqualityComparer<T>? comparer = null,
        bool isFinalBlock = true);

    public static OperationStatus Replace(
        this ReadOnlySpan<char> source,
        Span<char> destination,
        ReadOnlySpan<char> oldValue,
        ReadOnlySpan<char> newValue,
        out int charsConsumed,
        out int charsWritten,
        out int replacementCount,
        StringComparison comparison = StringComparison.Ordinal,
        bool isFinalBlock = true);
}
```
## Deprecate / Hide Environment.SpecialFolder.Personal

**Approved** | [#runtime/75563](https://github.com/dotnet/runtime/issues/75563#issuecomment-6022959511) | [Video](https://www.youtube.com/watch?v=g3vUhrs6yP4&t=1h13m53s)

* Personal/MyDocuments are literally the only two values with the same numeric value, so hiding the confusing one makes sense.
* Use the next available SYSLIB diagnostic so we can make a fixer
* Please, someone, make a fixer.
* EB-Never seems good.

```diff
namespace System;

public static partial class Environment
{
    public enum SpecialFolder
    {
+       [EditorBrowsable(EditorBrowsableState.Never)]
+       [Obsolete("Use Environment.SpecialFolder.MyDocuments instead", DiagnosticId=SYSLIB????)]
        Personal = SpecialFolderValues.CSIDL_PERSONAL,
    }
}
```
## Roslyn analyzer/fixer: Simplify invocations that receive a start/offset and a count/length

**Approved** | [#runtime/35981](https://github.com/dotnet/runtime/issues/35981#issuecomment-6023174871) | [Video](https://www.youtube.com/watch?v=g3vUhrs6yP4&t=1h34m52s)

* Let's use the same diagnostic for AsSpan(0, buf.Length) => AsSpan() and AsSpan(x, buf.Length - x) => AsSpan(x)
* Let's use the same diagnostic for all method groups (AsSpan, AsMemory, Slice, any others)
* Let's use a DIFFERENT diagnostic for AsSpan(..) (AsSpan(Range)), and all other range-syntax-targeting-Range-argument-based sites.

Category: Performance
Level: Suggestion
Off by default
## Add BitwiseAndNot To TensorPrimitives

**Approved** | [#runtime/105307](https://github.com/dotnet/runtime/issues/105307#issuecomment-6023443376) | [Video](https://www.youtube.com/watch?v=g3vUhrs6yP4&t=1h46m17s)

* BitwiseAndNot is elsewhere in the platform as just AndNot
* The arguments are normally (left, right) for AndNot, but everything else on Tensor/TensorPrimitives is x,y, so x,y there and left,right on IBitwiseOperators.
* We should also add the members on Tensor
* Math says we don't need TensorPrimitives.AndNot(span, scalar, destination), because that's BitwiseAnd(span, ~scalar, destination).

```csharp
 namespace System.Numerics.Tensors {
   public static partial class TensorPrimitives {
     public static void AndNot<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : System.Numerics.IBitwiseOperators<T, T, T>;
     public static void AndNot<T>(T x, ReadOnlySpan<T> y, Span<T> destination) where T : System.Numerics.IBitwiseOperators<T, T, T>;
   }

   public static partial class Tensor {
     public static ref readonly Tensor<T> AndNot<T>(
        in ReadOnlyTensorSpan<T> x,
        in ReadOnlyTensorSpan<T> y,
        in TensorSpan<T> destination)
        where T : IBitwiseOperators<T, T, T>;

     public static ref readonly Tensor<T> AndNot<T>(
        T x,
        in ReadOnlyTensorSpan<T> y,
        in TensorSpan<T> destination)
        where T : IBitwiseOperators<T, T, T>;

     public static ref readonly TensorSpan<T> AndNot<T>(
        scoped in ReadOnlyTensorSpan<T> x,
        scoped in ReadOnlyTensorSpan<T> y,
        in TensorSpan<T> destination)
        where T : IBitwiseOperators<T, T, T>;

      public static ref readonly TensorSpan<T> AndNot<T>(
        T x,
        scoped in ReadOnlyTensorSpan<T> y,
        in TensorSpan<T> destination)
        where T : IBitwiseOperators<T, T, T>;
   }
 }

 namespace System.Numerics {
   public partial interface IBitwiseOperators<TSelf,TOther,TResult> where TSelf : IBitwiseOperators<TSelf, TOther, TResult> {
     public static virtual TResult AndNot(TSelf left, TOther right);
   }
 }
```
