# API Review 11/14/2023

## Additional QuicConnectionOptions

**Approved** | [#runtime/72984](https://github.com/dotnet/runtime/issues/72984#issuecomment-1810933700) | [Video](https://www.youtube.com/watch?v=2NsF2PBS02w&t=0h0m0s)


* A brief discussion of making `KeepAliveInterval` nullable, but the consensus was to expose the Inifinite default.
* We factored out the `InitialLocallyInitiatedBidirectionalStreamReceiveWindowSize` (et al) properties into a struct both for togetherness and to reduce the name

```C#
namespace System.Net.Quic
{
    public struct QuicReceiveWindowSizes
    {
        /// <summary>
        /// The initial flow-control window size for the connection.
        /// </summary>
        public int Connection { get; set; } = 16 * 1024 * 1024;

        /// <summary>
        /// The initial flow-control window size for locally initiated bidirectional streams.
        /// </summary>
        public int LocallyInitiatedBidirectionalStream { get; set; } = 64 * 1024;

         /// <summary>
        /// The initial flow-control window size for remotely initiated bidirectional streams.
        /// </summary>
        public int RemotelyInitiatedBidirectionalStream { get; set; } = 64 * 1024;

        /// <summary>
        /// The initial flow-control window size for (remotely initiated) unidirectional streams.
        /// </summary>
        public int UnidirectionalStream { get; set; } = 64 * 1024;
    }

    public partial class QuicConnectionOptions
    {
        /// <summary>
        /// The interval at which keep alive packets are sent on the connection.
        /// Default <see cref="TimeSpan.Zero"/>, or <see cref="Timeout.InfiniteTimeSpan"> means no keep alive.
        /// </summary>
        public TimeSpan KeepAliveInterval { get; set; } = TimeSpan.Infinite;

        public QuicReceiveWindowSizes InitialReceiveWindowSizes { get; set; }
        
        /// <summary>
        /// The upper bound on time when the handshake must complete. If the handshake does not
        /// complete in this time, the connection is aborted.
        /// </summary>
        public TimeSpan HandshakeTimeout { get; set; } = TimeSpan.FromSeconds(10);
    }
}
```
## Make mutable generic collection interfaces implement read-only collection interfaces

**Approved** | [#runtime/31001](https://github.com/dotnet/runtime/issues/31001#issuecomment-1811013088) | [Video](https://www.youtube.com/watch?v=2NsF2PBS02w&t=0h40m6s)

It sounds like we're ready to try the experiment.  To everyone who is celebrating right now, remember: if it goes poorly we're going to back it out :smile:.

```diff
 namespace System.Collections.Generic {
-    public interface ICollection<T> : IEnumerable<T> {
+    public interface ICollection<T> : IReadOnlyCollection<T> {
-        int Count { get; }
+        new int Count { get; }
+        int IReadOnlyCollection<T>.Count => Count;
     }
-    public interface IList<T> : ICollection<T> {
+    public interface IList<T> : ICollection<T>, IReadOnlyList<T> {
-        T this[int index] { get; set; }
+        new T this[int index] { get; set; }
+        T IReadOnlyList<T>.this[int index] => this[index];
     }
-    public interface IDictionary<TKey, TValue> : ICollection<KeyValuePair<TKey, TValue>> {
+    public interface IDictionary<TKey, TValue> : ICollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue> {
-        TValue this[TKey key] { get; set; }
+        new TValue this[TKey key] { get; set; }
-        ICollection<TKey> Keys { get; }
+        new ICollection<TKey> Keys { get; }
-        ICollection<TValue> Values { get; }
+        new ICollection<TValue> Values { get; }
-        bool ContainsKey(TKey key);
+        new bool ContainsKey(TKey key);
-        bool TryGetValue(TKey key, out TValue value);
+        new bool TryGetValue(TKey key, out TValue value);
+        TValue IReadOnlyDictionary<TKey, TValue>.this[TKey key] => this[key];
+        IEnumerable<TKey> IReadOnlyDictionary<TKey, TValue>.Keys => Keys;
+        IEnumerable<TValue> IReadOnlyDictionary<TKey, TValue>.Values => Values;
+        bool IReadOnlyDictionary<TKey, TValue>.ContainsKey(TKey key) => ContainsKey(key);
+        bool IReadOnlyDictionary<TKey, TValue>.TryGetValue(TKey key, out TValue value) => TryGetValue(key, out value);
     }
-    public interface ISet<T> : ICollection<T> {
+    public interface ISet<T> : ICollection<T>, IReadOnlySet<T> {
-        bool IsProperSubsetOf(IEnumerable<T> other);
+        new bool IsProperSubsetOf(IEnumerable<T> other);
-        bool IsProperSupersetOf(IEnumerable<T> other);
+        new bool IsProperSupersetOf(IEnumerable<T> other);
-        bool IsSubsetOf(IEnumerable<T> other);
+        new bool IsSubsetOf(IEnumerable<T> other);
-        bool IsSupersetOf(IEnumerable<T> other);
+        new bool IsSupersetOf(IEnumerable<T> other);
-        bool Overlaps(IEnumerable<T> other);
+        new bool Overlaps(IEnumerable<T> other);
-        bool SetEquals(IEnumerable<T> other);
+        new bool SetEquals(IEnumerable<T> other);
// Adding this new member is required so that there's a most specific Contains method on ISet<T> since ICollection<T> and IReadOnlySet<T> define it too
+        new bool Contains(T value) => ((ICollection<T>)this).Contains(value); 
+        bool IReadOnlySet<T>.Contains(T value) => ((ICollection<T>)this).Contains(value);
+        bool IReadOnlySet<T>.IsProperSubsetOf(IEnumerable<T> other) => IsProperSubsetOf(other);
+        bool IReadOnlySet<T>.IsProperSupersetOf(IEnumerable<T> other) => IsProperSupersetOf(other);
+        bool IReadOnlySet<T>.IsSubsetOf(IEnumerable<T> other) => IsSubsetOf(other);
+        bool IReadOnlySet<T>.IsSupersetOf(IEnumerable<T> other) => IsSupersetOf(other);
+        bool IReadOnlySet<T>.Overlaps(IEnumerable<T> other) => Overlaps(other);
+        bool IReadOnlySet<T>.SetEquals(IEnumerable<T> other) => SetEquals(other);
+    }
 }
```
## Add overload of `Type.GetMethod` that takes a name, generic parameter count, binding flags and parameter types

**Approved** | [#runtime/90995](https://github.com/dotnet/runtime/issues/90995#issuecomment-1811031787) | [Video](https://www.youtube.com/watch?v=2NsF2PBS02w&t=1h5m25s)

Looks good as proposed.

```diff
 namespace System;

 public abstract partial class Type
 {
     public MethodInfo? GetMethod(string name, int genericParameterCount, Type[] types);
     public MethodInfo? GetMethod(string name, int genericParameterCount, Type[] types, ParameterModifier[]? modifiers);
+    public MethodInfo? GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Type[] types);
     public MethodInfo? GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
     public MethodInfo? GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
     public MethodInfo? GetMethod(string name);
     public MethodInfo? GetMethod(string name, Type[] types);
     public MethodInfo? GetMethod(string name, Type[] types, ParameterModifier[]? modifiers);
     public MethodInfo? GetMethod(string name, BindingFlags bindingAttr);
     public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Type[] types);
     public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
     public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
 }
```
## Add `BindingFlags.IgnoreAccessModifiers`

**Approved** | [#runtime/94536](https://github.com/dotnet/runtime/issues/94536#issuecomment-1811062092) | [Video](https://www.youtube.com/watch?v=2NsF2PBS02w&t=1h10m13s)

We're not sure that there's benefit in adding the enum member that isn't already (and better) covered by the proposed analyzer.  So, until/unless there's something to tip the scales in favor of adding it we feel like the new API should not be added but the analyzer is good.

Analyzer approved
Category: Maintainability
Severity: Warning
## Generic overloads of existing TensorPrimitives methods

**Approved** | [#runtime/94553](https://github.com/dotnet/runtime/issues/94553#issuecomment-1811099516) | [Video](https://www.youtube.com/watch?v=2NsF2PBS02w&t=1h30m22s)

Looks good as proposed (taking all of the constraints on faith)

```C#
namespace System.Numerics.Tensors;

public static partial class TensorPrimitives
{
    public static void Abs<T>(ReadOnlySpan<T> x, Span<T> destination) where T : INumberBase<T>;
    public static void AddMultiply<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, ReadOnlySpan<T> multiplier, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void AddMultiply<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, T multiplier, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void AddMultiply<T>(ReadOnlySpan<T> x, T y, ReadOnlySpan<T> multiplier, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void Add<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;
    public static void Add<T>(ReadOnlySpan<T> x, T y, Span<T> destination)  where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;
    public static void Cosh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunctions<T>;
    public static T CosineSimilarity<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : IRootFunctions<T>;
    public static T Distance<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : INumberBase<T>, IRootFunctions<T>;
    public static void Divide<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : IDivisionOperators<T, T, T>;
    public static void Divide<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IDivisionOperators<T, T, T>;
    public static T Dot<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>, IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static void Exp<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IExponentialFunctions<T>;
    public static int IndexOfMax<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMaxMagnitude<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMin<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMinMagnitude<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static void Log2<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
    public static void Log<T>(ReadOnlySpan<T> x, Span<T> destination) where T : ILogarithmicFunctions<T>;
    public static T MaxMagnitude<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static void MaxMagnitude<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumberBase<T>;
    public static T Max<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static void Max<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumber<T>;
    public static T MinMagnitude<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static void MinMagnitude<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumberBase<T>;
    public static T Min<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static void Min<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumber<T>;
    public static void MultiplyAdd<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, ReadOnlySpan<T> addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void MultiplyAdd<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, T addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void MultiplyAdd<T>(ReadOnlySpan<T> x, T y, ReadOnlySpan<T> addend, Span<T> destination) where T : IAdditionOperators<T, T, T>, IMultiplyOperators<T, T, T>;
    public static void Multiply<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumberBase<T>;
    public static void Multiply<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static void Negate<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IUnaryNegationOperators<T, T>;
    public static T Norm<T>(ReadOnlySpan<T> x) where T : IRootFunctions<T>;
    public static T ProductOfDifferences<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : ISubtractionOperators<T, T, T>, IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static T ProductOfSums<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>, IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static T Product<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static void Sigmoid<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IExponentialFunction<T>;
    public static void Sinh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunction<T>;
    public static void SoftMax<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IExpoentialFunction<T>;
    public static void Subtract<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : ISubtractionOperators<T, T, T>;
    public static void Subtract<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : ISubtractionOperators<T, T, T>;
    public static T SumOfMagnitudes<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static T SumOfSquares<T>(ReadOnlySpan<T> x) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>, IMultiplyOperators<T, T, T>;
    public static T Sum<T>(ReadOnlySpan<T> x) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;
    public static void Tanh<T>(ReadOnlySpan<T> x, Span<T> destination) where T : IHyperbolicFunctions<T>;
}
```
