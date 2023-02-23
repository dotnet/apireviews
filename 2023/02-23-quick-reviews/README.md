# API Review 02/23/2023

## Define a pattern for default values that used to be constants proportional to the default system font.

**Approved** | [#winforms/8599](https://github.com/dotnet/winforms/issues/8599#issuecomment-1442264796) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=0h0m0s)

Looks good as proposed.

We discussed whether the methods `ResetXxx()` and `ShouldSerializeXxx()` should be public. We concluded that they shouldn't be because they are indirectly used by `TypeDescriptor`, which is the way the designer (and other serializers) should interact with these types.

It looks like we might want to consider an analyzer that checks for an assignment of `ItemHeight` and instructs the user to delete the line. Alternatively, the designer could me made aware of which properties are DPI dependent and doesn't persist those values to hardcoded values into the designer generated code. It sounds like this is planned for a later day.
## Provide a built-in way to create APM methods from a Task-based method

**Approved** | [#runtime/61729](https://github.com/dotnet/runtime/issues/61729#issuecomment-1442291622) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=0h30m51s)

We concluded that having a separate type will be easier should we need to ship this downlevel (and there is some evidence that suggests we might want to do that).

```C#
namespace System.Threading.Tasks;

public static class TaskToAsyncResult
{
    public static IAsyncResult Begin(Task task, AsyncCallback? callback, object? state);
    public static void End(IAsyncResult asyncResult);
    public static TResult End<TResult>(IAsyncResult asyncResult);
    public static Task Unwrap(IAsyncResult asyncResult);
    public static Task<TResult> Unwrap<TResult>(IAsyncResult asyncResult);
}
```
## Arm VectorTableLookup and VectorTableExtension - Part 2

**Approved** | [#runtime/81599](https://github.com/dotnet/runtime/issues/81599#issuecomment-1442308902) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=0h48m43s)

* Looks good as proposed.
    - There seem to be some copy & paste error where some `sbyte` should be `byte`

```C#
namespace System.Runtime.Intrinsics.Arm;

public partial class AdvSimd
{
    public static Vector64<byte>  VectorTableLookup((Vector128<byte> Row0, Vector128<byte> Row1)  table, Vector64<byte>  byteIndexes);
    public static Vector64<sbyte> VectorTableLookup((Vector128<sbyte> Row0, Vector128<byte> Row1) table, Vector64<sbyte> byteIndexes);

    public static Vector64<byte>  VectorTableLookupExtension(Vector64<byte> defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1) table, Vector64<byte>  byteIndexes);
    public static Vector64<sbyte> VectorTableLookupExtension(Vector64<byte> defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1) table, Vector64<sbyte> byteIndexes);

    public partial class Arm32
    {
        public static Vector64<byte>  VectorTableLookup ((Vector64<byte>  Row0, Vector64<byte>  Row1) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookup ((Vector64<sbyte> Row0, Vector64<sbyte> Row1) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookup ((Vector64<byte>  Row0, Vector64<byte>  Row1, Vector64<byte>  Row2) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookup ((Vector64<sbyte> Row0, Vector64<sbyte> Row1, Vector64<sbyte> Row2) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookup ((Vector64<byte>  Row0, Vector64<byte>  Row1, Vector64<byte>  Row2, Vector64<byte>  Row3) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookup ((Vector64<sbyte> Row0, Vector64<sbyte> Row1, Vector64<sbyte> Row2, Vector64<sbyte> Row3) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<byte>  Row0, Vector64<byte>  Row1) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<sbyte> Row0, Vector64<sbyte> Row1) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<byte>  Row0, Vector64<byte>  Row1,Vector64<byte>  Row2) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<sbyte> Row0, Vector64<sbyte> Row1,Vector64<sbyte> Row2) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<byte>  Row0, Vector64<byte>  Row1, Vector64<byte> Row2, Vector64<byte>  Row3) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte> VectorTableLookupExtension (Vector64<byte> defaultValues, (Vector64<sbyte> Row0, Vector64<sbyte> Row1, Vector64<sbyte>Row2, Vector64<sbyte> Row3) table, Vector64<sbyte> byteIndexes);
    }

    public partial class Arm64
    {
        public static Vector64<byte>   VectorTableLookup ((Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte>  VectorTableLookup ((Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>   VectorTableLookup ((Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2, Vector128<byte>  Row3) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte>  VectorTableLookup ((Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2, Vector128<sbyte> Row3) table, Vector64<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookup ((Vector128<byte>  Row0, Vector128<byte>  Row1) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookup ((Vector128<sbyte> Row0, Vector128<sbyte> Row1) table, Vector128<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookup ((Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookup ((Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2) table, Vector128<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookup ((Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2, Vector128<byte>  Row3) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookup ((Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2, Vector128<sbyte> Row3) table, Vector128<sbyte> byteIndexes);

        public static Vector64<byte>   VectorTableLookupExtension (Vector64<byte>  defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte>  VectorTableLookupExtension (Vector64<byte>  defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2) table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>   VectorTableLookupExtension (Vector64<byte>  defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2, Vector128<byte>  Row3) table, Vector64<byte> byteIndexes);
        public static Vector64<sbyte>  VectorTableLookupExtension (Vector64<byte>  defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2, Vector128<sbyte> Row3) table, Vector64<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1) table, Vector128<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2) table, Vector128<sbyte> byteIndexes);

        public static Vector128<byte>  VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<byte>  Row0, Vector128<byte>  Row1, Vector128<byte>  Row2, Vector128<byte>  Row3) table, Vector128<byte> byteIndexes);
        public static Vector128<sbyte> VectorTableLookupExtension (Vector128<byte> defaultValues, (Vector128<sbyte> Row0, Vector128<sbyte> Row1, Vector128<sbyte> Row2, Vector128<sbyte> Row3) table, Vector128<sbyte> byteIndexes);
    }
}
```
## Expose method on PeriodicTimer to adjust interval

**Approved** | [#runtime/60384](https://github.com/dotnet/runtime/issues/60384#issuecomment-1442313341) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h4m32s)

* Looks good as proposed

```C#
namespace System.Threading;

public partial class PeriodicTimer
{
    public TimeSpan Period { get; set; }
}
```
## `Unsafe.BitCast` for efficient type reinterpretation

**Approved** | [#runtime/81334](https://github.com/dotnet/runtime/issues/81334#issuecomment-1442326543) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h9m3s)

* Looks good as proposed
   - We should constrain it to `struct` because reference types should use `As`

```C#
namespace System.Runtime.CompilerServices;

public static class Unsafe
{
    public static TTo BitCast<TFrom, TTo>(TFrom value) where TFrom: struct, TTo: struct;
}
```
## Add ECDiffieHellman.DeriveSecretAgreement method

**Approved** | [#runtime/71613](https://github.com/dotnet/runtime/issues/71613#issuecomment-1442335941) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h22m7s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

public partial class ECDiffieHellman : ECAlgorithm
{
    public virtual byte[] DeriveRawSecretAgreement(ECDiffieHellmanPublicKey otherPartyPublicKey);
}
```
## [Analyzer] Upgrade from platform specific to cross platform intrinsics

**Approved** | [#runtime/82486](https://github.com/dotnet/runtime/issues/82486#issuecomment-1442344454) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h29m13s)

Code fixer looks good as proposed.

This would handle most common operators (`+`, `-`, `/`, `*`, and even `Xor`).

Either category seems fine, but we have a slight preference for maintainability.

```diff
- Sse.Add(Sse.Multiply(left, right), addend)
+ (left * right) + addend
```

## RSA.GetMaxOutputSize

**Approved** | [#runtime/78175](https://github.com/dotnet/runtime/issues/78175#issuecomment-1442353853) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h34m14s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography;

public partial class RSA
{
    public int GetMaxOutputSize();
}
```

## [Analyzer] Suggest more optimal SIMD patterns where applicable

**Approved** | [#runtime/82488](https://github.com/dotnet/runtime/issues/82488#issuecomment-1442360824) | [Video](https://www.youtube.com/watch?v=lYGQ8Sg_23w&t=1h39m14s)

Category: **Performance**

For code that replaces calls into intrinsics this seems fine, such as:

```diff
- Sse.Shuffle(value, value, 0b11_11_10_10);
+ Sse.UnpackHigh(value, value);
```

For more general code that doesn't involve calls to intrinsics we should consider recognizing those in the JIT instead, such as:

```diff
- value * 2;
+ value + value;
```

