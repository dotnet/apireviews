# ASN.1 BER/CER/DER Reader & Writer

**In Progress** |
[Spec](https://github.com/dotnet/designs/pull/93) |
[Video](https://youtu.be/PtiTXxroJT4)

## Round 1

* We're reading data that is encoded in that format, as opposed to the
  description language itself. In XML speak, we're building an
  `XmlReader`/`XmlWriter` as opposed to `XmlSchema`.
* Why only a `ref struct` reader? Because `ref struct` writers are error prone
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
