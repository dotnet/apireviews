# API Review 03/18/2025

## Type Mapping API

**Approved** | [#runtime/113362](https://github.com/dotnet/runtime/issues/113362#issuecomment-2734273949) | [Video](https://www.youtube.com/watch?v=rXDXsAhvBDw&t=0h0m0s)


* TTypeUniverse => TTypeMapGroup
* The string in the RUCAttribute could use some love.

```C#
namespace System.Runtime.InteropServices;

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple = true)]
public sealed class TypeMapAttribute<TTypeMapGroup> : Attribute
{
    public TypeMapAttribute(string value, Type target) { }

    [RequiresUnreferencedCode("Interop types may be removed by trimming")]
    public TypeMapAttribute(string value, Type target, Type trimTarget) { }
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple = true)]
public sealed class TypeMapAssemblyTargetAttribute<TTypeMapGroup> : Attribute
{
    public TypeMapAssemblyTargetAttribute(string assemblyName) { }
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple = false)]
public sealed class TypeMapAssociationAttribute<TTypeMapGroup> : Attribute
{
    public TypeMapAssociationAttribute(Type source, Type proxy) { }
}

public static class TypeMapping
{
    [RequiresUnreferencedCode("Interop types may be removed by trimming")]
    public static IReadOnlyDictionary<string, Type> GetOrCreateExternalTypeMapping<TTypeMapGroup>();

    [RequiresUnreferencedCode("Interop types may be removed by trimming")]
    public static IReadOnlyDictionary<Type, Type> GetOrCreateProxyTypeMapping<TTypeMapGroup>();
}
```
## System.Formats.Asn1.AsnValueReader

**Approved** | [#runtime/109019](https://github.com/dotnet/runtime/issues/109019#issuecomment-2734312504) | [Video](https://www.youtube.com/watch?v=rXDXsAhvBDw&t=1h5m11s)

* Consider whether it really needs to be public (that is, if it has utility outside dotnet/runtime)
* If it does need to be public, `Value` should be a prefix, not an infix.

```C#
namespace System.Formats.Asn1;

public partial ref struct ValueAsnReader {
    public ValueAsnReader(ReadOnlySpan<byte> data, AsnEncodingRules ruleSet, AsnReaderOptions options = default);
    public readonly bool HasData { get; }
    public readonly AsnEncodingRules RuleSet { get; }

    // Clone is not needed since AsnValueReader is a struct and can be cloned with copy-by-value.
    // We can have it for consistency sake, if we really want.
    //public AsnValueReader Clone();

    public readonly ReadOnlySpan<byte> PeekContentBytes();
    public readonly ReadOnlySpan<byte> PeekEncodedValue();
    public readonly Asn1Tag PeekTag();
    public readonly void ThrowIfNotEmpty();

    public byte[] ReadBitString(out int unusedBitCount, Asn1Tag? expectedTag = default);
    public bool TryReadPrimitiveBitString(out int unusedBitCount, out ReadOnlySpan<byte> value, Asn1Tag? expectedTag = default);
    public bool TryReadBitString(Span<byte> destination, out int unusedBitCount, out int bytesWritten, Asn1Tag? expectedTag = default);

    public bool ReadBoolean(Asn1Tag? expectedTag = default);

    public string ReadCharacterString(UniversalTagNumber encodingType, Asn1Tag? expectedTag = default);
    public bool TryReadCharacterString(Span<char> destination, UniversalTagNumber encodingType, out int charsWritten, Asn1Tag? expectedTag = default);
    public bool TryReadCharacterStringBytes(Span<byte> destination, Asn1Tag expectedTag, out int bytesWritten);
    public bool TryReadPrimitiveCharacterStringBytes(Asn1Tag expectedTag, out ReadOnlySpan<byte> contents);

    public ReadOnlySpan<byte> ReadEncodedValue();

    public ReadOnlySpan<byte> ReadEnumeratedBytes(Asn1Tag? expectedTag = default);
    public Enum ReadEnumeratedValue(Type enumType, Asn1Tag? expectedTag = default);
    public TEnum ReadEnumeratedValue<TEnum>(Asn1Tag? expectedTag = default) where TEnum : Enum;

    public DateTimeOffset ReadGeneralizedTime(Asn1Tag? expectedTag = default);

    public BigInteger ReadInteger(Asn1Tag? expectedTag = default);
    public ReadOnlySpan<byte> ReadIntegerBytes(Asn1Tag? expectedTag = default);
    public bool TryReadInt32(out int value, Asn1Tag? expectedTag = default);
    public bool TryReadInt64(out long value, Asn1Tag? expectedTag = default);
    [CLSCompliantAttribute(false)]
    public bool TryReadUInt32(out uint value, Asn1Tag? expectedTag = default);
    [CLSCompliantAttribute(false)]
    public bool TryReadUInt64(out ulong value, Asn1Tag? expectedTag = default);

    public BitArray ReadNamedBitList(Asn1Tag? expectedTag = default);
    public Enum ReadNamedBitListValue(Type flagsEnumType, Asn1Tag? expectedTag = default);
    public TFlagsEnum ReadNamedBitListValue<TFlagsEnum>(Asn1Tag? expectedTag = default) where TFlagsEnum : Enum;
    public void ReadNull(Asn1Tag? expectedTag = default);

    public string ReadObjectIdentifier(Asn1Tag? expectedTag = default);

    public byte[] ReadOctetString(Asn1Tag? expectedTag = default);
    public bool TryReadOctetString(Span<byte> destination, out int bytesWritten, Asn1Tag? expectedTag = default);
    public bool TryReadPrimitiveOctetString(out ReadOnlySpan<byte> contents, Asn1Tag? expectedTag = default);

    public ValueAsnReader ReadSequence(Asn1Tag? expectedTag = default);
    public ValueAsnReader ReadSetOf(bool skipSortOrderValidation, Asn1Tag? expectedTag = default);
    public ValueAsnReader ReadSetOf(Asn1Tag? expectedTag = default);
    public DateTimeOffset ReadUtcTime(int twoDigitYearMax, Asn1Tag? expectedTag = default);
    public DateTimeOffset ReadUtcTime(Asn1Tag? expectedTag = default);
}
```
## System.IO.Hashing.Adler32 class for computing Adler32 checksums

**Approved** | [#runtime/90191](https://github.com/dotnet/runtime/issues/90191#issuecomment-2734347547) | [Video](https://www.youtube.com/watch?v=rXDXsAhvBDw&t=1h8m7s)

* The API shape looks good as proposed.
* As a feature review, we should get a commitment/intent from someone (e.g. ImageSharp) to use our version once it exists.  We shouldn't do it in a vacuum.

```C#
namespace System.IO.Hashing;

    public sealed partial class Adler32 : System.IO.Hashing.NonCryptographicHashAlgorithm
    {
        public Adler32();
        public override void Append(System.ReadOnlySpan<byte> source);
        public Adler32 Clone();
        public uint GetCurrentHashAsUInt32();
        protected override void GetCurrentHashCore(System.Span<byte> destination);
        protected override void GetHashAndResetCore(System.Span<byte> destination);
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(System.ReadOnlySpan<byte> source);
        public static int Hash(System.ReadOnlySpan<byte> source, System.Span<byte> destination);
        public static uint HashToUInt32(System.ReadOnlySpan<byte> source);
        public override void Reset();
        public static bool TryHash(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    }
```
## Expose span on SpanSplitEnumerator

**Approved** | [#runtime/109874](https://github.com/dotnet/runtime/issues/109874#issuecomment-2734364233) | [Video](https://www.youtube.com/watch?v=rXDXsAhvBDw&t=1h23m20s)

* The new member should be a get-only property, not a field.
* The new member should be named Source, both so it's not ever confused with Current and to match the name of the parameter when it was constructed.

```C#
public ref partial struct SpanSplitEnumerator<T>
{
   public ReadOnlySpan<T> Source { readonly get; }
}
```
