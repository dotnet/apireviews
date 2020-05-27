# ASN.1 BER/CER/DER Reader & Writer

**Approved** |
[Spec](https://github.com/dotnet/designs/pull/93)


## Round 1

[Video](https://youtu.be/PtiTXxroJT4)

* We're reading data that is encoded in that format, as opposed to the
  description language itself. In XML speak, we're building an
  `XmlReader`/`XmlWriter` as opposed to `XmlSchema`.
* Why a `ref struct` reader but no `ref struct` writer? Because `ref struct` writers are error prone
  to use, see JSON writer
* `Asn1Tag` looks good as proposed
* `TagClass` should extend `int`
* `UniversalTagNumber` looks good as proposed
* `AsnEncodingRules` looks good as proposed
* `AsnWriter`
  * Should we make it non-disposable? We can make it disposable before we
      ship.
  * Should the ctor also take an initial capacity?
  * `GetEncodedLength()` should throw `InvalidOperationException`, rather than
      returning `-1`
  * `ValueEquals()` should be `EncodedValueEquals()`
* What namespace should this go in?
  * `System.Protocols.Asn1`?
  * `System.Protocols` would be the home for protocols that aren't important
    enough to be promoted elsewhere (such as  XML, JSON, HTTP).
* Next: https://github.com/dotnet/designs/blob/894b79a4361145a3037cf70fdf50689e88983a1b/accepted/2020/asnreader/asnreader.md#data-injection


## Round 2

[Video](https://www.youtube.com/watch?v=-qetGrkYAjk)

Covered https://github.com/dotnet/designs/blob/894b79a4361145a3037cf70fdf50689e88983a1b/accepted/2020/asnreader/asnreader.md#data-injection through all of AsnWriter and AsnReader. (Stopped at https://github.com/dotnet/designs/blob/894b79a4361145a3037cf70fdf50689e88983a1b/accepted/2020/asnreader/asnreader.md#asnvaluereader-api)

### AsnWriter

* WriteEncodedValue should throw ArgumentException, not CryptographicException, when the value is not valid.
* Change WriteEncodedValue's parameter to "value"
* General: Make Asn1Tag overloads take it as a defaulted nullable; collapse overloads.
* Ensure all WriteInteger methods have tag-based inputs (probably was a copy/paste error).
* WriteBitString: Change an "unused" bit being "1" to ArgumentException.
* WriteBitString: Consider adding a BitArray overload (which doesn't need an unusedBitCount parameter)
  * (WriteNamedBitList may be a better target)
* WriteBitString: Remove the SpanAction overloads.
* WriteOctetString: Remove the SpanAction overloads.
* WriteObjectIdentifier: Remove `Oid`-typed overloads.
* WriteEnumeratedValue: Change `object enumValue` to `Enum enumValue`
* WriteEnumeratedValue: Rename `enumValue` parameters to `value`.
* WriteNamedBitList: Like WriteEnumeratedValue, change to `Enum enumValue` and rename all `enumValue` parameters to `value`.
* WriteNamedBitList: Consider adding a BitArray overload.

### AsnReader
* Remove Oid-typed methods, simplify to `public string ReadObjectIdentifier(Asn1Tag? tag = default)`
* Move all tag parameters to defaulted nullables.
* TryReadInt*: drop 16 and 8
* Rename all Copy methods to Read methods
* Consider adding `public BitArray ReadNameBitListValue()`
* ReadGeneralizedTime: remove disallowFractions
* TryCopyCharacterStringBytes: does the overload with expectedTag and encodingType need both parameters? If not, remove the `UniversalTagNumber encodingType` parameter.

### AsnValueReader

* Consider replacing the mutable `ref struct` with a non-ref struct that only has stateless, static methods.

## Round 3

[Video](https://www.youtube.com/watch?v=CmF8g030Hno)

* `AsnDecoder`
    - Would benefit from `Range` but needs to be .NET Standard 2.0, where `Range` doesn't exist
    - We should consider OOBing the type
* `AsnReaderOptions`
    - Remove `AsnEncodingRules`
* `Push`/`Pop`
    - It's a bit odd to have both disposable `Scope` that pops and a `Pop` method
    - Seems fine though
