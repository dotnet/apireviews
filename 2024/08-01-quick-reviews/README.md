# API Review 08/01/2024

## Expand ExchangeAlgorithmType, CipherAlgorithmType, HashAlgorithmType

**Approved** | [#runtime/100361](https://github.com/dotnet/runtime/issues/100361#issuecomment-2263603056) | [Video](https://www.youtube.com/watch?v=vZ49KRsLcvs&t=0h0m0s)

* Instead of modifying the enums, we're going to mark them as Obsolete and direct people to the TlsCipherSuite instead.
* All of these obsoleted members should use the same diagnostic ID (whatever SYSLIB is next)

```diff
namespace System.Security.Authentication
{
+   [Obsolete]
    public enum ExchangeAlgorithmType { }

+   [Obsolete]
    public enum CipherAlgorithmType { }

+   [Obsolete]
    public enum HashAlgorithmType { }
}

namespace System.Net.Security
{
+   [Obsolete]
    public virtual ExchangeAlgorithmType KeyExchangeAlgorithm { get; }

+   [Obsolete]
    public virtual int KeyExchangeStrength { get; }

+   [Obsolete]
    public virtual CipherAlgorithmType CipherAlgorithm { get; }

+   [Obsolete]
    public virtual int CipherStrength { get; }    

+   [Obsolete]
    public virtual HashAlgorithmType HashAlgorithm { get; }

+   [Obsolete]
    public virtual int HashStrength { get; }
}
```
## Simplify lookup of parsed TypeName in metadata

**Approved** | [#runtime/101774](https://github.com/dotnet/runtime/issues/101774#issuecomment-2263693421) | [Video](https://www.youtube.com/watch?v=vZ49KRsLcvs&t=0h20m3s)

* System.Type.Namespace is a nullable string property, and seems to have distinction between `null` (Generic type parameters) and empty (global namespace types); it was asked if we need the same semantics here.  The answer was "no, this will always return empty".
* It was asked if we need a `static string Escape(string name)` for symmetry, and the answer was "no, call the CreateSimpleTypeName API and then read the appropriate name property from the result".

```c#
namespace System.Reflection.Metadata;

public partial class TypeName
{      
    public string Namespace { get; }

    public static string Unescape(string name);
}
```
## Creating new TypeName without reparsing

**Approved** | [#runtime/102263](https://github.com/dotnet/runtime/issues/102263#issuecomment-2263669399) | [Video](https://www.youtube.com/watch?v=vZ49KRsLcvs&t=1h8m24s)

* WithAssemblyName doesn't seem to have a solid scenario (outside of one case), so it's being withheld until it has a more clear use pattern with regard to composed types (e.g. generics).
* It was asked if the instance methods should be prefixed with "Make" or "Create", and it seems that "Make" matches reflection, and the static one "Create" is a patterned prefix.
* We changed CreateSimpleTypeName's `fullName` parameter to `metadataName` to avoid confusion between the unescaped value as a parameter and the escaped value as the property with the same name.
* The parameterless MakeArrayTypeName is renamed to MakeSZArrayTypeName because MakeArrayTypeName(1) and MakeArrayTypeName() are not the same.

```c#
namespace System.Reflection.Metadata;

public partial class TypeName
{      
    public TypeName MakeSZArrayTypeName();
    public TypeName MakeArrayTypeName(int rank);
    public TypeName MakePointerTypeName();
    public TypeName MakeByRefTypeName();
    public TypeName MakeGenericTypeName(ImmutableArray<TypeName> typeArguments);

    public static TypeName CreateSimpleTypeName(
        string metadataName,
        TypeName? declaringType = null,
        AssemblyNameInfo? assemblyInfo = null);
}
```
## Additional vectorized functions

**Approved** | [#runtime/103845](https://github.com/dotnet/runtime/issues/103845#issuecomment-2263757811) | [Video](https://www.youtube.com/watch?v=vZ49KRsLcvs&t=1h8m48s)

* The ClampNative/MinNative/MaxNative are being made explicit on our three floating point types
* Otherwise, looks good as proposed, modulo some typos

```c#
namespace System
{
    public partial struct Single
    {
        public static float ClampNative(float x, float min, float max);
        public static float MaxNative(float x, float y);
        public static float MinNative(float x, float y);
    }

    public partial struct Double
    {
        public static float ClampNative(double x, double min, double max);
        public static float MaxNative(double x, double y);
        public static float MinNative(double x, double y);
    }

    public partial struct Half
    {
        public static float ClampNative(Half x, Half min, Half max);
        public static float MaxNative(Half x, Half y);
        public static float MinNative(Half x, Half y);
    }
}

namespace System.Numerics
{
    public partial interface INumber<TSelf>
    {
        public static virtual TSelf ClampNative(TSelf x, TSelf min, TSelf max);
        public static virtual TSelf MaxNative(TSelf x, TSelf y);
        public static virtual TSelf MinNative(TSelf x, TSelf y);
    }

    public static partial class Vector
    {
        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector<T> IsEvenInteger<T>(Vector<T> x);
        public static Vector<T> IsFinite<T>(Vector<T> x);
        public static Vector<T> IsInfinity<T>(Vector<T> x);
        public static Vector<T> IsInteger<T>(Vector<T> x);
        public static Vector<T> IsNaN<T>(Vector<T> x);
        public static Vector<T> IsNegative<T>(Vector<T> x);
        public static Vector<T> IsNegativeInfinity<T>(Vector<T> x);
        public static Vector<T> IsNormal<T>(Vector<T> x);
        public static Vector<T> IsOddInteger<T>(Vector<T> x);
        public static Vector<T> IsPositive<T>(Vector<T> x);
        public static Vector<T> IsPositiveInfinity<T>(Vector<T> x);
        public static Vector<T> IsSubnormal<T>(Vector<T> x);
        public static Vector<T> IsZero<T>(Vector<T> x);

        public static Vector<T> ClampNative<T>(Vector<T> x, Vector<T> min, Vector<T> max);
        public static Vector<T> MaxNative<T>(Vector<T> x, Vector<T> y);
        public static Vector<T> MinNative<T>(Vector<T> x, Vector<T> y);

        public static bool All<T>(Vector<T> vector, T value);
        public static bool AllWhereAllBitsSet<T>(Vector<T> vector);

        public static bool None<T>(Vector<T> vector, T value);
        public static bool NoneWhereAllBitsSet<T>(Vector<T> vector);

        public static void Store(this Vector2 source, float* destination);
        public static void StoreAligned(this Vector2 source, float* destination);
        public static void StoreAlignedNonTemporal(this Vector2 source, float* destination);
        public static void StoreUnsafe(this Vector2 source, ref float destination);
        public static void StoreUnsafe(this Vector2 source, ref float destination, nuint elementOffset);

        public static void Store(this Vector3 source, float* destination);
        public static void StoreAligned(this Vector3 source, float* destination);
        public static void StoreAlignedNonTemporal(this Vector3 source, float* destination);
        public static void StoreUnsafe(this Vector3 source, ref float destination);
        public static void StoreUnsafe(this Vector3 source, ref float destination, nuint elementOffset);

        public static void Store(this Vector4 source, float* destination);
        public static void StoreAligned(this Vector4 source, float* destination);
        public static void StoreAlignedNonTemporal(this Vector4 source, float* destination);
        public static void StoreUnsafe(this Vector4 source, ref float destination);
        public static void StoreUnsafe(this Vector4 source, ref float destination, nuint elementOffset);
    }
    
    public partial struct Vector2
    {
        public static Vector2 AllBitsSet { get; }
        public static float this[int index] { get; set; }

        public static Vector2 operator &(Vector2 left, Vector2 right);
        public static Vector2 operator |(Vector2 left, Vector2 right);
        public static Vector2 operator ^(Vector2 left, Vector2 right);
        public static Vector2 operator <<(Vector2 value, int shiftAmount);
        public static Vector2 operator ~(Vector2 value);
        public static Vector2 operator >>(Vector2 value, int shiftAmount);
        public static Vector2 operator +(Vector2 value);
        public static Vector2 operator >>>(Vector2 value, int shiftAmount);

        public static Vector2 AndNot(Vector2 left, Vector2 right);
        public static Vector2 BitwiseAnd(Vector2 left, Vector2 right);
        public static Vector2 BitwiseOr(Vector2 left, Vector2 right);
        public static Vector2 ConditionalSelect(Vector2 condition, Vector2 left, Vector2 right);
        public static Vector2 OnesComplement(Vector2 value);
        public static Vector2 Xor(Vector2 left, Vector2 right);

        public static Vector2 ClampNative(Vector2 x, Vector2 min, Vector2 max);
        public static Vector2 MaxNative(Vector2 x, Vector2 y);
        public static Vector2 MinNative(Vector2 x, Vector2 y);

        public static Vector2 Equals(Vector2 left, Vector2 right);
        public static bool EqualsAll(Vector2 left, Vector2 right);
        public static bool EqualsAny(Vector2 left, Vector2 right);

        public static int ExtractMostSignificantBits(Vector2 vector);

        public static float GetElement(this Vector2 vector, int index);
        public static float ToScalar(this Vector2 vector);
        public static Vector2 WithElement(this Vector2 vector, int index, float value);

        public static Vector2 GreaterThan(Vector2 left, Vector2 right);
        public static bool GreaterThanAll(Vector2 left, Vector2 right);
        public static bool GreaterThanAny(Vector2 left, Vector2 right);

        public static Vector2 GreaterThanOrEqual(Vector2 left, Vector2 right);
        public static bool GreaterThanOrEqualAll(Vector2 left, Vector2 right);
        public static bool GreaterThanOrEqualAny(Vector2 left, Vector2 right);

        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector2 IsEvenInteger(Vector2 x);
        public static Vector2 IsFinite(Vector2 x);
        public static Vector2 IsInfinity(Vector2 x);
        public static Vector2 IsInteger(Vector2 x);
        public static Vector2 IsNaN(Vector2 x);
        public static Vector2 IsNegative(Vector2 x);
        public static Vector2 IsNegativeInfinity(Vector2 x);
        public static Vector2 IsNormal(Vector2 x);
        public static Vector2 IsOddInteger(Vector2 x);
        public static Vector2 IsPositive(Vector2 x);
        public static Vector2 IsPositiveInfinity(Vector2 x);
        public static Vector2 IsSubnormal(Vector2 x);
        public static Vector2 IsZero(Vector2 x);

        public static Vector2 LessThan(Vector2 left, Vector2 right);
        public static bool LessThanAll(Vector2 left, Vector2 right);
        public static bool LessThanAny(Vector2 left, Vector2 right);

        public static Vector2 LessThanOrEqual(Vector2 left, Vector2 right);
        public static bool LessThanOrEqualAll(Vector2 left, Vector2 right);
        public static bool LessThanOrEqualAny(Vector2 left, Vector2 right);

        public static Vector2 Load(float* source);
        public static Vector2 LoadAligned(float* source);
        public static Vector2 LoadAlignedNonTemporal(float* source);
        public static Vector2 LoadUnsafe(ref float source);
        public static Vector2 LoadUnsafe(ref float source, nuint elementOffset);

        public static Vector2 Shuffle(Vector2 vector, byte xIndex, byte yIndex);

        public static float Sum(Vector2 vector);

        public static bool Any(Vector2 vector, T value);
        public static bool AnyWhereAllBitsSet(Vector2 vector);

        public static bool All(Vector2 vector, T value);
        public static bool AllWhereAllBitsSet(Vector2 vector);

        public static bool None(Vector2 vector, T value);
        public static bool NoneWhereAllBitsSet(Vector2 vector);

        public static int Count(Vector2 vector, T value);
        public static int CountWhereAllBitsSet(Vector2 vector);

        public static int IndexOf(Vector2 vector, T value);
        public static int IndexOfWhereAllBitsSet(Vector2 vector);
        
        public static int LastIndexOf(Vector2 vector, T value);
        public static int LastIndexOfWhereAllBitsSet(Vector2 vector);
    }
    
    public partial struct Vector3
    {
        public static Vector3 AllBitsSet { get; }
        public static float this[int index] { get; set; }

        public static Vector3 operator &(Vector3 left, Vector3 right);
        public static Vector3 operator |(Vector3 left, Vector3 right);
        public static Vector3 operator ^(Vector3 left, Vector3 right);
        public static Vector3 operator <<(Vector3 value, int shiftAmount);
        public static Vector3 operator ~(Vector3 value);
        public static Vector3 operator >>(Vector3 value, int shiftAmount);
        public static Vector3 operator +(Vector3 value);
        public static Vector3 operator >>>(Vector3 value, int shiftAmount);

        public static Vector3 AndNot(Vector3 left, Vector3 right);
        public static Vector3 BitwiseAnd(Vector3 left, Vector3 right);
        public static Vector3 BitwiseOr(Vector3 left, Vector3 right);
        public static Vector3 ConditionalSelect(Vector3 condition, Vector3 left, Vector3 right);
        public static Vector3 OnesComplement(Vector3 value);
        public static Vector3 Xor(Vector3 left, Vector3 right);

        public static Vector3 ClampNative(Vector3 x, Vector3 min, Vector3 max);
        public static Vector3 MaxNative(Vector3 x, Vector3 y);
        public static Vector3 MinNative(Vector3 x, Vector3 y);

        public static Vector3 Equals(Vector3 left, Vector3 right);
        public static bool EqualsAll(Vector3 left, Vector3 right);
        public static bool EqualsAny(Vector3 left, Vector3 right);

        public static int ExtractMostSignificantBits(Vector3 vector);

        public static float GetElement(this Vector3 vector, int index);
        public static float ToScalar(this Vector3 vector);
        public static Vector3 WithElement(this Vector3 vector, int index, float value);

        public static Vector3 GreaterThan(Vector3 left, Vector3 right);
        public static bool GreaterThanAll(Vector3 left, Vector3 right);
        public static bool GreaterThanAny(Vector3 left, Vector3 right);

        public static Vector3 GreaterThanOrEqual(Vector3 left, Vector3 right);
        public static bool GreaterThanOrEqualAll(Vector3 left, Vector3 right);
        public static bool GreaterThanOrEqualAny(Vector3 left, Vector3 right);

        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector3 IsEvenInteger(Vector3 x);
        public static Vector3 IsFinite(Vector3 x);
        public static Vector3 IsInfinity(Vector3 x);
        public static Vector3 IsInteger(Vector3 x);
        public static Vector3 IsNaN(Vector3 x);
        public static Vector3 IsNegative(Vector3 x);
        public static Vector3 IsNegativeInfinity(Vector3 x);
        public static Vector3 IsNormal(Vector3 x);
        public static Vector3 IsOddInteger(Vector3 x);
        public static Vector3 IsPositive(Vector3 x);
        public static Vector3 IsPositiveInfinity(Vector3 x);
        public static Vector3 IsSubnormal(Vector3 x);
        public static Vector3 IsZero(Vector3 x);

        public static Vector3 LessThan(Vector3 left, Vector3 right);
        public static bool LessThanAll(Vector3 left, Vector3 right);
        public static bool LessThanAny(Vector3 left, Vector3 right);

        public static Vector3 LessThanOrEqual(Vector3 left, Vector3 right);
        public static bool LessThanOrEqualAll(Vector3 left, Vector3 right);
        public static bool LessThanOrEqualAny(Vector3 left, Vector3 right);

        public static Vector3 Load(float* source);
        public static Vector3 LoadAligned(float* source);
        public static Vector3 LoadAlignedNonTemporal(float* source);
        public static Vector3 LoadUnsafe(ref float source);
        public static Vector3 LoadUnsafe(ref float source, nuint elementOffset);

        public static Vector3 Shuffle(Vector3 vector, byte xIndex, byte yIndex, byte zIndex);

        public static float Sum(Vector3 vector);

        public static bool Any(Vector3 vector, T value);
        public static bool AnyWhereAllBitsSet(Vector3 vector);

        public static bool All(Vector3 vector, T value);
        public static bool AllWhereAllBitsSet(Vector3 vector);

        public static bool None(Vector3 vector, T value);
        public static bool NoneWhereAllBitsSet(Vector3 vector);

        public static int Count(Vector3 vector, T value);
        public static int CountWhereAllBitsSet(Vector3 vector);

        public static int IndexOf(Vector3 vector, T value);
        public static int IndexOfWhereAllBitsSet(Vector3 vector);
        
        public static int LastIndexOf(Vector3 vector, T value);
        public static int LastIndexOfWhereAllBitsSet(Vector3 vector);
    }
    
    public partial struct Vector4
    {
        public static Vector4 AllBitsSet { get; }
        public static float this[int index] { get; set; }

        public static Vector4 operator &(Vector4 left, Vector4 right);
        public static Vector4 operator |(Vector4 left, Vector4 right);
        public static Vector4 operator ^(Vector4 left, Vector4 right);
        public static Vector4 operator <<(Vector4 value, int shiftAmount);
        public static Vector4 operator ~(Vector4 value);
        public static Vector4 operator >>(Vector4 value, int shiftAmount);
        public static Vector4 operator +(Vector4 value);
        public static Vector4 operator >>>(Vector4 value, int shiftAmount);

        public static Vector4 AndNot(Vector4 left, Vector4 right);
        public static Vector4 BitwiseAnd(Vector4 left, Vector4 right);
        public static Vector4 BitwiseOr(Vector4 left, Vector4 right);
        public static Vector4 ConditionalSelect(Vector4 condition, Vector4 left, Vector4 right);
        public static Vector4 OnesComplement(Vector4 value);
        public static Vector4 Xor(Vector4 left, Vector4 right);

        public static Vector4 ClampNative(Vector4 x, Vector4 min, Vector4 max);
        public static Vector4 MaxNative(Vector4 x, Vector4 y);
        public static Vector4 MinNative(Vector4 x, Vector4 y);

        public static Vector4 Equals(Vector4 left, Vector4 right);
        public static bool EqualsAll(Vector4 left, Vector4 right);
        public static bool EqualsAny(Vector4 left, Vector4 right);

        public static int ExtractMostSignificantBits(Vector4 vector);

        public static float GetElement(this Vector4 vector, int index);
        public static float ToScalar(this Vector4 vector);
        public static Vector4 WithElement(this Vector4 vector, int index, float value);

        public static Vector4 GreaterThan(Vector4 left, Vector4 right);
        public static bool GreaterThanAll(Vector4 left, Vector4 right);
        public static bool GreaterThanAny(Vector4 left, Vector4 right);

        public static Vector4 GreaterThanOrEqual(Vector4 left, Vector4 right);
        public static bool GreaterThanOrEqualAll(Vector4 left, Vector4 right);
        public static bool GreaterThanOrEqualAny(Vector4 left, Vector4 right);

        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector4 IsEvenInteger(Vector4 x);
        public static Vector4 IsFinite(Vector4 x);
        public static Vector4 IsInfinity(Vector4 x);
        public static Vector4 IsInteger(Vector4 x);
        public static Vector4 IsNaN(Vector4 x);
        public static Vector4 IsNegative(Vector4 x);
        public static Vector4 IsNegativeInfinity(Vector4 x);
        public static Vector4 IsNormal(Vector4 x);
        public static Vector4 IsOddInteger(Vector4 x);
        public static Vector4 IsPositive(Vector4 x);
        public static Vector4 IsPositiveInfinity(Vector4 x);
        public static Vector4 IsSubnormal(Vector4 x);
        public static Vector4 IsZero(Vector4 x);

        public static Vector4 LessThan(Vector4 left, Vector4 right);
        public static bool LessThanAll(Vector4 left, Vector4 right);
        public static bool LessThanAny(Vector4 left, Vector4 right);

        public static Vector4 LessThanOrEqual(Vector4 left, Vector4 right);
        public static bool LessThanOrEqualAll(Vector4 left, Vector4 right);
        public static bool LessThanOrEqualAny(Vector4 left, Vector4 right);

        public static Vector4 Load(float* source);
        public static Vector4 LoadAligned(float* source);
        public static Vector4 LoadAlignedNonTemporal(float* source);
        public static Vector4 LoadUnsafe(ref float source);
        public static Vector4 LoadUnsafe(ref float source, nuint elementOffset);

        public static Vector4 Shuffle(Vector4 vector, byte xIndex, byte yIndex, byte zIndex, byte wIndex);

        public static float Sum(Vector4 vector);

        public static bool Any(Vector4 vector, T value);
        public static bool AnyWhereAllBitsSet(Vector4 vector);

        public static bool All(Vector4 vector, T value);
        public static bool AllWhereAllBitsSet(Vector4 vector);

        public static bool None(Vector4 vector, T value);
        public static bool NoneWhereAllBitsSet(Vector4 vector);

        public static int Count(Vector4 vector, T value);
        public static int CountWhereAllBitsSet(Vector4 vector);

        public static int IndexOf(Vector4 vector, T value);
        public static int IndexOfWhereAllBitsSet(Vector4 vector);
        
        public static int LastIndexOf(Vector4 vector, T value);
        public static int LastIndexOfWhereAllBitsSet(Vector4 vector);
    }
}

namespace System.Runtime.Intrinsics
{
    public static partial class Vector64
    {
        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector64<T> IsEvenInteger<T>(Vector64<T> x);
        public static Vector64<T> IsFinite<T>(Vector64<T> x);
        public static Vector64<T> IsInfinity<T>(Vector64<T> x);
        public static Vector64<T> IsInteger<T>(Vector64<T> x);
        public static Vector64<T> IsNaN<T>(Vector64<T> x);
        public static Vector64<T> IsNegative<T>(Vector64<T> x);
        public static Vector64<T> IsNegativeInfinity<T>(Vector64<T> x);
        public static Vector64<T> IsNormal<T>(Vector64<T> x);
        public static Vector64<T> IsOddInteger<T>(Vector64<T> x);
        public static Vector64<T> IsPositive<T>(Vector64<T> x);
        public static Vector64<T> IsPositiveInfinity<T>(Vector64<T> x);
        public static Vector64<T> IsSubnormal<T>(Vector64<T> x);
        public static Vector64<T> IsZero<T>(Vector64<T> x);

        public static Vector64<T> ClampNative<T>(Vector64<T> x, Vector64<T> min, Vector64<T> max);
        public static Vector64<T> MaxNative<T>(Vector64<T> x, Vector64<T> y);
        public static Vector64<T> MinNative<T>(Vector64<T> x, Vector64<T> y);

        public static bool All<T>(Vector64<T> vector, T value);
        public static bool AllWhereAllBitsSet<T>(Vector64<T> vector);

        public static bool None<T>(Vector64<T> vector, T value);
        public static bool NoneWhereAllBitsSet<T>(Vector64<T> vector);
    }

    public static partial class Vector128
    {
        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector128<T> IsEvenInteger<T>(Vector128<T> x);
        public static Vector128<T> IsFinite<T>(Vector128<T> x);
        public static Vector128<T> IsInfinity<T>(Vector128<T> x);
        public static Vector128<T> IsInteger<T>(Vector128<T> x);
        public static Vector128<T> IsNaN<T>(Vector128<T> x);
        public static Vector128<T> IsNegative<T>(Vector128<T> x);
        public static Vector128<T> IsNegativeInfinity<T>(Vector128<T> x);
        public static Vector128<T> IsNormal<T>(Vector128<T> x);
        public static Vector128<T> IsOddInteger<T>(Vector128<T> x);
        public static Vector128<T> IsPositive<T>(Vector128<T> x);
        public static Vector128<T> IsPositiveInfinity<T>(Vector128<T> x);
        public static Vector128<T> IsSubnormal<T>(Vector128<T> x);
        public static Vector128<T> IsZero<T>(Vector128<T> x);

        public static Vector128<T> ClampNative<T>(Vector128<T> x, Vector128<T> min, Vector128<T> max);
        public static Vector128<T> MaxNative<T>(Vector128<T> x, Vector128<T> y);
        public static Vector128<T> MinNative<T>(Vector128<T> x, Vector128<T> y);

        public static bool All<T>(Vector128<T> vector, T value);
        public static bool AllWhereAllBitsSet<T>(Vector128<T> vector);

        public static bool None<T>(Vector128<T> vector, T value);
        public static bool NoneWhereAllBitsSet<T>(Vector128<T> vector);
    }

    public static partial class Vector256
    {
        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector256<T> IsEvenInteger<T>(Vector256<T> x);
        public static Vector256<T> IsFinite<T>(Vector256<T> x);
        public static Vector256<T> IsInfinity<T>(Vector256<T> x);
        public static Vector256<T> IsInteger<T>(Vector256<T> x);
        public static Vector256<T> IsNaN<T>(Vector256<T> x);
        public static Vector256<T> IsNegative<T>(Vector256<T> x);
        public static Vector256<T> IsNegativeInfinity<T>(Vector256<T> x);
        public static Vector256<T> IsNormal<T>(Vector256<T> x);
        public static Vector256<T> IsOddInteger<T>(Vector256<T> x);
        public static Vector256<T> IsPositive<T>(Vector256<T> x);
        public static Vector256<T> IsPositiveInfinity<T>(Vector256<T> x);
        public static Vector256<T> IsSubnormal<T>(Vector256<T> x);
        public static Vector256<T> IsZero<T>(Vector256<T> x);

        public static Vector256<T> ClampNative<T>(Vector256<T> x, Vector256<T> min, Vector256<T> max);
        public static Vector256<T> MaxNative<T>(Vector256<T> x, Vector256<T> y);
        public static Vector256<T> MinNative<T>(Vector256<T> x, Vector256<T> y);

        public static bool All<T>(Vector256<T> vector, T value);
        public static bool AllWhereAllBitsSet<T>(Vector256<T> vector);

        public static bool None<T>(Vector256<T> vector, T value);
        public static bool NoneWhereAllBitsSet<T>(Vector256<T> vector);
    }

    public static partial class Vector512
    {
        // Do not expose:
        // * IsCanonical, always true
        // * IsComplexNumber, always false
        // * IsImaginaryNumber, always false
        // * IsRealNumber, always true

        public static Vector512<T> IsEvenInteger<T>(Vector512<T> x);
        public static Vector512<T> IsFinite<T>(Vector512<T> x);
        public static Vector512<T> IsInfinity<T>(Vector512<T> x);
        public static Vector512<T> IsInteger<T>(Vector512<T> x);
        public static Vector512<T> IsNaN<T>(Vector512<T> x);
        public static Vector512<T> IsNegative<T>(Vector512<T> x);
        public static Vector512<T> IsNegativeInfinity<T>(Vector512<T> x);
        public static Vector512<T> IsNormal<T>(Vector512<T> x);
        public static Vector512<T> IsOddInteger<T>(Vector512<T> x);
        public static Vector512<T> IsPositive<T>(Vector512<T> x);
        public static Vector512<T> IsPositiveInfinity<T>(Vector512<T> x);
        public static Vector512<T> IsSubnormal<T>(Vector512<T> x);
        public static Vector512<T> IsZero<T>(Vector512<T> x);

        public static Vector512<T> ClampNative<T>(Vector512<T> x, Vector512<T> min, Vector512<T> max);
        public static Vector512<T> MaxNative<T>(Vector512<T> x, Vector512<T> y);
        public static Vector512<T> MinNative<T>(Vector512<T> x, Vector512<T> y);

        public static bool All<T>(Vector512<T> vector, T value);
        public static bool AllWhereAllBitsSet<T>(Vector512<T> vector);

        public static bool None<T>(Vector512<T> vector, T value);
        public static bool NoneWhereAllBitsSet<T>(Vector512<T> vector);
    }
}
```
