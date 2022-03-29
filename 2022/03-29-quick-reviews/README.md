# API Review 03/29/2022

## Updating the generic math draft proposal to include changes proposed for .NET 7

**NeedsWork** | [#designs/257](https://github.com/dotnet/designs/pull/257#issuecomment-1082307333) | [Video](https://www.youtube.com/watch?v=Qo40CuHxSwk&t=0h0m0s)

API Review notes:

* Let's move all the interfaces except IParseable and ISpanParseable to System.Numerics.
* A quick English language check says `IParsable` is the correct spelling over `IParseable`, and this matches the W3C's guidance and Wiktionary.
* INumber should probably stay INumber.  INumberBase may be the best name, but we don't super like it.  An alternative was `INonComparableNumber`.
* All of the members on the current INumber move down to the base interface except for Clamp, Max, and Min (the things that need comparisons)
* ISignedNumberBase and IUnsignedNumberBase should drop their suffixes and drop their inheritance (just be marker interfaces)
* IBinaryInteger
    * Rename (Try)CopyTo to (Try)WriteLittleEndian.
    * The non-Try versions should return int.
    * The non-Try versions should be written as DIMs
    * GetByteCount should return `int`
    * GetBitLength should return `long`.
* IFloatingPoint<TSelf>.Round<TInteger> should just lock in Int32 and not be double-generic.
* IFloatingPointIeee754<TSelf>
    * PI should change to `Pi`.
    * FusedMultiplyAdd should update the parameter names (left, right, add) in the proposal.
    * IEEERemainder => IeeeRemainder (or Ieee754Remainder)
    * CopyExponentTo => WriteExponentLittleEndian (and return types, and DIMs)
    * GetByteCount should return `int`
    * GetBitLength should return `long`.
* Let's go with the CreateChecked(TOther) + CreateChecked(TOther, TNumberConverter) overload pairs plan.


