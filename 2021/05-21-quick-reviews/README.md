# Quick Reviews 05/21/2021

## All inherited interface members should be implemented on an interface marked with DynamicInterfaceCastableImplementationAttribute

**Approved** | [#runtime/41529](https://github.com/dotnet/runtime/issues/41529#issuecomment-846114993) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=0h0m0s)

The analyzer looks good as proposed.

Regarding Error vs Warning: @terrajobst can you chime in with what the heuristics are?  If there's a mode where we can be an on-by-default Error we think that might be the right answer.
## Provide JsonContent<T> type that uses pre-generated serialization metadata

**Approved** | [#runtime/51544](https://github.com/dotnet/runtime/issues/51544#issuecomment-846126667) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=0h13m49s)

* Because the JsonContent type is currently sealed with a non-public constructor there's more flexibility and we can add the `JsonContent<T> : JsonContent` relationship.
* The JsonContent.Create that takes a TValue parameter should be declared generic (typo).

```C#
namespace System.Net.Http.Json
{
    public sealed partial class JsonContent<TValue> : JsonContent
    {
        internal JsonContent() { }
        public TValue? Value { get { throw null; } }
    }
    partial class JsonContent
    {
        public static JsonContent<object?> Create(object? inputValue, Type inputType, JsonSerializerContext context, MediaTypeHeaderValue? mediaType = null) { throw null; }
        public static JsonContent<TValue> Create<TValue>(TValue? inputValue, JsonTypeInfo<TValue> jsonTypeInfo, MediaTypeHeaderValue? mediaType = null) { throw null; }
    }
}
```

```diff
namespace System.Net.Http.Json
{
-    public sealed partial class JsonContent
+    public abstract partial class JsonContent
     {
        internal JsonContent();
     }
+   internal sealed partial class ReflectionBasedJsonContent  : JsonContent { ... }
}
```
## string.NormalizeLineEndings

**Approved** | [#runtime/40315](https://github.com/dotnet/runtime/issues/40315#issuecomment-846147770) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=0h34m39s)

* Instead of default parameter, make a parameterless overload and make the parameter non-nullable
* The word "Normalize" is overloaded in this space, suggest ReplaceLineEndings instead of NormalizeLineEndings
* Suggest `replacementText` instead of `lineEnding` in the ReplaceLineEndings method.
* If a char overload seems reasonable, go ahead and add it, too.

```C#
namespace System
{
    partial class String
    {
        public string ReplaceLineEndings() => ReplaceLineEndings(Environment.NewLine);
        public string ReplaceLineEndings(string replacementText);
    }
    partial class MemoryExtensions
    {
        public System.Text.SpanLineEnumerator EnumerateLines(this ReadOnlySpan<char> span);
        public System.Text.SpanLineEnumerator EnumerateLines(this Span<char> span);
    }
}
namespace System.Text
{
    public ref struct SpanLineEnumerator
    {
        public ReadOnlySpan<char> Current;
        public SpanLineEnumerator GetEnumerator();
        public bool MoveNext();
    }
}
```
## Add ThreadPoolBoundHandle.AllocateUnsafeNativeOverlapped

**Approved** | [#runtime/42549](https://github.com/dotnet/runtime/issues/42549#issuecomment-846152662) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=1h13m38s)

* For pattern-matching, the `Unsafe` should be a prefix instead of an infix.
* PreAllocatedOverlapped.UnsafeCreate makes sense to add at the same time.

```C#
namespace System.Threading
{
    partial class ThreadPoolBoundHandle
    {
       [CLSCompliant(false)]
       public unsafe NativeOverlapped* UnsafeAllocateNativeOverlapped(IOCompletionCallback callback, object? state, object? pinData);
    }
}
namespace System.Threading
{
    partial class PreAllocatedOverlapped : IDisposable
    {
        [CLSCompliant(false)]
        public static PreAllocatedOverlapped UnsafeCreate(IOCompletionCallback callback, object? state, object? pinData);
    }
}
```

## Add API to find MethodInfo on instantiated generic type from generic type definition

**Approved** | [#runtime/45771](https://github.com/dotnet/runtime/issues/45771#issuecomment-846169871) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=1h24m30s)

* We feel that the name "GetMemberWithSameMetadataDefinitionAs" does a very good job of explaining that it's the same as HasSameMetadataDefinitionAs, but from a different perspective.

```C#
namespace System
{
    partial class Type
    {
        public virtual MemberInfo? GetMemberWithSameMetadataDefinitionAs(MemberInfo member);
    }
}
```
## Add public key methods for S.S.C.X509Certificates.PublicKey

**Approved** | [#runtime/48510](https://github.com/dotnet/runtime/issues/48510#issuecomment-846171514) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=1h46m45s)

Looks good as proposed

```diff
namespace System.Security.Cryptography.X509Certificates
{
    public class PublicKey
    {
+       public RSA? GetRSAPublicKey();
+       public ECDsa? GetECDsaPublicKey();
+       public DSA? GetDSAPublicKey();
+       public ECDiffieHellman? GetECDiffieHellmanPublicKey();
    }
}
```
## Expose cross-platform helpers for Vector64, Vector128, and Vector256

**Approved** | [#runtime/49397](https://github.com/dotnet/runtime/issues/49397#issuecomment-846184811) | [Video](https://www.youtube.com/watch?v=8bnQoGUMXgM&t=1h50m10s)

* Looks good as proposed, modulo typos.
* There are some names that are changed (SquareRoot => Sqrt) and some members moved from instance to extension, but both of these changes make sense with recent API in similar scopes.

```C#
namespace System.Runtime.Intrinsics
{
    public static partial class Vector64
    {
        public bool IsHardwareAccelerated { get; }

        public static Vector64<T> Abs(Vector64<T> value);
        public static Vector64<T> Negate(Vector64<T> value);
        public static Vector64<T> OnesComplement(Vector64<T> value);
        public static Vector64<T> Sqrt(Vector64<T> value); // SquareRoot

        public static Vector64<T> Add(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> Subtract(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> Multiply(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> Divide(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> Dot(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> AndNot(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> BitwiseAnd(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> BitwiseOr(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> Xor(Vector64<T> left, Vector64<T> right);

        public static Vector64<double> Ceiling(Vector64<double> value);
        public static Vector64<float> Ceiling(Vector64<float> value);

        public static Vector64<double> Floor(Vector64<double> value);
        public static Vector64<float> Floor(Vector64<float> value);

        public static Vector64<T> ConditionalSelect(Vector64<T> condition, Vector64<T> left, Vector64<T> right);

        public static Vector64<double> ConvertToDouble(Vector64<long> value);
        public static Vector64<double> ConvertToDouble(Vector64<ulong> value);

        public static Vector64<int> ConvertToInt32(Vector64<float> value);
        public static Vector64<long> ConvertToInt64(Vector64<double> value);

        public static Vector64<float> ConvertToSingle(Vector64<int> value);
        public static Vector64<float> ConvertToSingle(Vector64<uint> value);

        public static Vector64<uint> ConvertToUInt32(Vector64<float> value);
        public static Vector64<ulong> ConvertToUInt64(Vector64<double> value);

        public static Vector64<T> Equals(Vector64<T> left, Vector64<T> right);

        public static bool EqualsAll(Vector64<T> left, Vector64<T> right);
        public static bool EqualsAny(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> GreaterThan(Vector64<T> left, Vector64<T> right);

        public static bool GreaterThanAll(Vector64<T> left, Vector64<T> right);
        public static bool GreaterThanAny(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> GreaterThanOrEqual(Vector64<T> left, Vector64<T> right);

        public static bool GreaterThanOrEqualAll(Vector64<T> left, Vector64<T> right);
        public static bool GreaterThanOrEqualAny(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> LessThan(Vector64<T> left, Vector64<T> right);

        public static bool LessThanAll(Vector64<T> left, Vector64<T> right);
        public static bool LessThanAny(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> LessThanOrEqual(Vector64<T> left, Vector64<T> right);

        public static bool LessThanOrEqualAll(Vector64<T> left, Vector64<T> right);
        public static bool LessThanOrEqualAny(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> Max(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> Min(Vector64<T> left, Vector64<T> right);

        public static Vector64<float> Narrow(Vector64<double> lower, Vector64<T> upper);
        public static Vector64<sbyte> Narrow(Vector64<short> lower, Vector64<short> upper);
        public static Vector64<short> Narrow(Vector64<int> lower, Vector64<int> upper);
        public static Vector64<int> Narrow(Vector64<long> lower, Vector64<long> upper);
        public static Vector64<byte> Narrow(Vector64<ushort> lower, Vector64<ushort> upper);
        public static Vector64<ushort> Narrow(Vector64<uint> lower, Vector64<uint> upper);
        public static Vector64<uint> Narrow(Vector64<ulong> lower, Vector64<ulong> upper);

        public static (Vector64<short> Lower, Vector64<short> Upper) Widen(Vector64<sbyte> value);
        public static (Vector64<int> Lower, Vector64<int> Upper) Widen(Vector64<short> value);
        public static (Vector64<long> Lower, Vector64<long> Upper) Widen(Vector64<int> value);
        public static (Vector64<double> Lower, Vector64<double> Upper) Widen(Vector64<flaot> value);
        public static (Vector64<ushort> Lower, Vector64<ushort> Upper) Widen(Vector64<byte> value);
        public static (Vector64<uint> Lower, Vector64<uint> Upper) Widen(Vector64<ushort> value);
        public static (Vector64<ulong> Lower, Vector64<ulong> Upper) Widen(Vector64<uint> value);

        public static Vector64<T> Create<T>(ReadOnlySpan<byte> values);
        public static Vector64<T> Create<T>(ReadOnlySpan<T> values);
        public static Vector64<T> Create<T>(T[] values);
        public static Vector64<T> Create<T>(T[] values, int index);

        public static void CopyTo<T>(this Vector64<T> vector, Span<byte> destination);
        public static void CopyTo<T>(this Vector64<T> vector, Span<T> destination);
        public static void CopyTo<T>(this Vector64<T> vector, T[] destination);
        public static void CopyTo<T>(this Vector64<T> vector, T[] destination, int index);

        public static bool TryCopyTo(this Vector64<T> vector, Span<byte> destination);
        public static bool TryCopyTo(this Vector64<T> vector, Span<T> destination);
    }

    public partial struct Vector64<T>
        where T : struct
    {
        public static Vector64<T> operator +(Vector64<T> value);
        public static Vector64<T> operator -(Vector64<T> value);

        public static Vector64<T> operator ~(Vector64<T> value);

        public static bool operator ==(Vector64<T> left, Vector64<T> right);
        public static bool operator !=(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> operator +(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> operator -(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> operator *(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> operator /(Vector64<T> left, Vector64<T> right);

        public static Vector64<T> operator &(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> operator |(Vector64<T> left, Vector64<T> right);
        public static Vector64<T> operator ^(Vector64<T> left, Vector64<T> right);
    }
    public static partial class Vector128
    {
        public bool IsHardwareAccelerated { get; }

        public static Vector128<T> Abs(Vector128<T> value);
        public static Vector128<T> Negate(Vector128<T> value);
        public static Vector128<T> OnesComplement(Vector128<T> value);
        public static Vector128<T> Sqrt(Vector128<T> value); // SquareRoot

        public static Vector128<T> Add(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> Subtract(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> Multiply(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> Divide(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> Dot(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> AndNot(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> BitwiseAnd(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> BitwiseOr(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> Xor(Vector128<T> left, Vector128<T> right);

        public static Vector128<double> Ceiling(Vector128<double> value);
        public static Vector128<float> Ceiling(Vector128<float> value);

        public static Vector128<double> Floor(Vector128<double> value);
        public static Vector128<float> Floor(Vector128<float> value);

        public static Vector128<T> ConditionalSelect(Vector128<T> condition, Vector128<T> left, Vector128<T> right);

        public static Vector128<double> ConvertToDouble(Vector128<long> value);
        public static Vector128<double> ConvertToDouble(Vector128<ulong> value);

        public static Vector128<int> ConvertToInt32(Vector128<float> value);
        public static Vector128<long> ConvertToInt64(Vector128<double> value);

        public static Vector128<float> ConvertToSingle(Vector128<int> value);
        public static Vector128<float> ConvertToSingle(Vector128<uint> value);

        public static Vector128<uint> ConvertToUInt32(Vector128<float> value);
        public static Vector128<ulong> ConvertToUInt64(Vector128<double> value);

        public static Vector128<T> Equals(Vector128<T> left, Vector128<T> right);

        public static bool EqualsAll(Vector128<T> left, Vector128<T> right);
        public static bool EqualsAny(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> GreaterThan(Vector128<T> left, Vector128<T> right);

        public static bool GreaterThanAll(Vector128<T> left, Vector128<T> right);
        public static bool GreaterThanAny(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> GreaterThanOrEqual(Vector128<T> left, Vector128<T> right);

        public static bool GreaterThanOrEqualAll(Vector128<T> left, Vector128<T> right);
        public static bool GreaterThanOrEqualAny(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> LessThan(Vector128<T> left, Vector128<T> right);

        public static bool LessThanAll(Vector128<T> left, Vector128<T> right);
        public static bool LessThanAny(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> LessThanOrEqual(Vector128<T> left, Vector128<T> right);

        public static bool LessThanOrEqualAll(Vector128<T> left, Vector128<T> right);
        public static bool LessThanOrEqualAny(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> Max(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> Min(Vector128<T> left, Vector128<T> right);

        public static Vector128<float> Narrow(Vector128<double> lower, Vector128<T> upper);
        public static Vector128<sbyte> Narrow(Vector128<short> lower, Vector128<short> upper);
        public static Vector128<short> Narrow(Vector128<int> lower, Vector128<int> upper);
        public static Vector128<int> Narrow(Vector128<long> lower, Vector128<long> upper);
        public static Vector128<byte> Narrow(Vector128<ushort> lower, Vector128<ushort> upper);
        public static Vector128<ushort> Narrow(Vector128<uint> lower, Vector128<uint> upper);
        public static Vector128<uint> Narrow(Vector128<ulong> lower, Vector128<ulong> upper);

        public static (Vector128<short> Lower, Vector128<short> Upper) Widen(Vector128<sbyte> value);
        public static (Vector128<int> Lower, Vector128<int> Upper) Widen(Vector128<short> value);
        public static (Vector128<long> Lower, Vector128<long> Upper) Widen(Vector128<int> value);
        public static (Vector128<double> Lower, Vector128<double> Upper) Widen(Vector128<flaot> value);
        public static (Vector128<ushort> Lower, Vector128<ushort> Upper) Widen(Vector128<byte> value);
        public static (Vector128<uint> Lower, Vector128<uint> Upper) Widen(Vector128<ushort> value);
        public static (Vector128<ulong> Lower, Vector128<ulong> Upper) Widen(Vector128<uint> value);

        public static Vector128<T> Create<T>(ReadOnlySpan<byte> values);
        public static Vector128<T> Create<T>(ReadOnlySpan<T> values);
        public static Vector128<T> Create<T>(T[] values);
        public static Vector128<T> Create<T>(T[] values, int index);

        public static void CopyTo<T>(this Vector128<T> vector, Span<byte> destination);
        public static void CopyTo<T>(this Vector128<T> vector, Span<T> destination);
        public static void CopyTo<T>(this Vector128<T> vector, T[] destination);
        public static void CopyTo<T>(this Vector128<T> vector, T[] destination, int index);

        public static bool TryCopyTo(this Vector128<T> vector, Span<byte> destination);
        public static bool TryCopyTo(this Vector128<T> vector, Span<T> destination);
    }

    public partial struct Vector128<T>
        where T : struct
    {
        public static Vector128<T> operator +(Vector128<T> value);
        public static Vector128<T> operator -(Vector128<T> value);

        public static Vector128<T> operator ~(Vector128<T> value);

        public static bool operator ==(Vector128<T> left, Vector128<T> right);
        public static bool operator !=(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> operator +(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> operator -(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> operator *(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> operator /(Vector128<T> left, Vector128<T> right);

        public static Vector128<T> operator &(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> operator |(Vector128<T> left, Vector128<T> right);
        public static Vector128<T> operator ^(Vector128<T> left, Vector128<T> right);
    }
    public static partial class Vector256
    {
        public bool IsHardwareAccelerated { get; }

        public static Vector256<T> Abs(Vector256<T> value);
        public static Vector256<T> Negate(Vector256<T> value);
        public static Vector256<T> OnesComplement(Vector256<T> value);
        public static Vector256<T> Sqrt(Vector256<T> value); // SquareRoot

        public static Vector256<T> Add(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> Subtract(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> Multiply(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> Divide(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> Dot(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> AndNot(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> BitwiseAnd(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> BitwiseOr(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> Xor(Vector256<T> left, Vector256<T> right);

        public static Vector256<double> Ceiling(Vector256<double> value);
        public static Vector256<float> Ceiling(Vector256<float> value);

        public static Vector256<double> Floor(Vector256<double> value);
        public static Vector256<float> Floor(Vector256<float> value);

        public static Vector256<T> ConditionalSelect(Vector256<T> condition, Vector256<T> left, Vector256<T> right);

        public static Vector256<double> ConvertToDouble(Vector256<long> value);
        public static Vector256<double> ConvertToDouble(Vector256<ulong> value);

        public static Vector256<int> ConvertToInt32(Vector256<float> value);
        public static Vector256<long> ConvertToInt64(Vector256<double> value);

        public static Vector256<float> ConvertToSingle(Vector256<int> value);
        public static Vector256<float> ConvertToSingle(Vector256<uint> value);

        public static Vector256<uint> ConvertToUInt32(Vector256<float> value);
        public static Vector256<ulong> ConvertToUInt64(Vector256<double> value);

        public static Vector256<T> Equals(Vector256<T> left, Vector256<T> right);

        public static bool EqualsAll(Vector256<T> left, Vector256<T> right);
        public static bool EqualsAny(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> GreaterThan(Vector256<T> left, Vector256<T> right);

        public static bool GreaterThanAll(Vector256<T> left, Vector256<T> right);
        public static bool GreaterThanAny(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> GreaterThanOrEqual(Vector256<T> left, Vector256<T> right);

        public static bool GreaterThanOrEqualAll(Vector256<T> left, Vector256<T> right);
        public static bool GreaterThanOrEqualAny(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> LessThan(Vector256<T> left, Vector256<T> right);

        public static bool LessThanAll(Vector256<T> left, Vector256<T> right);
        public static bool LessThanAny(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> LessThanOrEqual(Vector256<T> left, Vector256<T> right);

        public static bool LessThanOrEqualAll(Vector256<T> left, Vector256<T> right);
        public static bool LessThanOrEqualAny(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> Max(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> Min(Vector256<T> left, Vector256<T> right);

        public static Vector256<float> Narrow(Vector256<double> lower, Vector256<T> upper);
        public static Vector256<sbyte> Narrow(Vector256<short> lower, Vector256<short> upper);
        public static Vector256<short> Narrow(Vector256<int> lower, Vector256<int> upper);
        public static Vector256<int> Narrow(Vector256<long> lower, Vector256<long> upper);
        public static Vector256<byte> Narrow(Vector256<ushort> lower, Vector256<ushort> upper);
        public static Vector256<ushort> Narrow(Vector256<uint> lower, Vector256<uint> upper);
        public static Vector256<uint> Narrow(Vector256<ulong> lower, Vector256<ulong> upper);

        public static (Vector256<short> Lower, Vector256<short> Upper) Widen(Vector256<sbyte> value);
        public static (Vector256<int> Lower, Vector256<int> Upper) Widen(Vector256<short> value);
        public static (Vector256<long> Lower, Vector256<long> Upper) Widen(Vector256<int> value);
        public static (Vector256<double> Lower, Vector256<double> Upper) Widen(Vector256<flaot> value);
        public static (Vector256<ushort> Lower, Vector256<ushort> Upper) Widen(Vector256<byte> value);
        public static (Vector256<uint> Lower, Vector256<uint> Upper) Widen(Vector256<ushort> value);
        public static (Vector256<ulong> Lower, Vector256<ulong> Upper) Widen(Vector256<uint> value);

        public static Vector256<T> Create<T>(ReadOnlySpan<byte> values);
        public static Vector256<T> Create<T>(ReadOnlySpan<T> values);
        public static Vector256<T> Create<T>(T[] values);
        public static Vector256<T> Create<T>(T[] values, int index);

        public static void CopyTo<T>(this Vector256<T> vector, Span<byte> destination);
        public static void CopyTo<T>(this Vector256<T> vector, Span<T> destination);
        public static void CopyTo<T>(this Vector256<T> vector, T[] destination);
        public static void CopyTo<T>(this Vector256<T> vector, T[] destination, int index);

        public static bool TryCopyTo(this Vector256<T> vector, Span<byte> destination);
        public static bool TryCopyTo(this Vector256<T> vector, Span<T> destination);
    }

    public partial struct Vector256<T>
        where T : struct
    {
        public static Vector256<T> operator +(Vector256<T> value);
        public static Vector256<T> operator -(Vector256<T> value);

        public static Vector256<T> operator ~(Vector256<T> value);

        public static bool operator ==(Vector256<T> left, Vector256<T> right);
        public static bool operator !=(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> operator +(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> operator -(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> operator *(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> operator /(Vector256<T> left, Vector256<T> right);

        public static Vector256<T> operator &(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> operator |(Vector256<T> left, Vector256<T> right);
        public static Vector256<T> operator ^(Vector256<T> left, Vector256<T> right);
    }
}
```
