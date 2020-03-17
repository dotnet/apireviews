# Quick Reviews 3/17/2020

## ARM additional arithmetic intrinsics

**Approved** | [#runtime/32512](https://github.com/dotnet/runtime/issues/32512#issuecomment-600227857)

* Looks good
* We made some substantial changing in order of words
* ~~`AbsoluteDifferenceAdd` in #24794 should take `Vector128<T>` rather than `Vector64<T>`~~
    - Actually it's correct
* The `Halving` methods are fused, so we should prefix them with `Fused`

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public partial class AdvSimd
    {
        public Vector64<byte>   AddLowerReturningUpper(Vector64<ushort> left, Vector64<ushort> right);
        public Vector64<sbyte>  AddLowerReturningUpper(Vector64<short>  left, Vector64<short>  right);
        public Vector64<short>  AddLowerReturningUpper(Vector64<int>    left, Vector64<int>    right);
        public Vector64<ushort> AddLowerReturningUpper(Vector64<uint>   left, Vector64<uint>   right);
        public Vector64<int>    AddLowerReturningUpper(Vector64<long>   left, Vector64<long>   right);
        public Vector64<uint>   AddLowerReturningUpper(Vector64<ulong>  left, Vector64<ulong>  right);

        public Vector64<byte>   AddLowerRoundedReturningUpper(Vector64<ushort> left, Vector64<ushort> right);
        public Vector64<sbyte>  AddLowerRoundedReturningUpper(Vector64<short>  left, Vector64<short>  right);
        public Vector64<short>  AddLowerRoundedReturningUpper(Vector64<int>    left, Vector64<int>    right);
        public Vector64<ushort> AddLowerRoundedReturningUpper(Vector64<uint>   left, Vector64<uint>   right);
        public Vector64<int>    AddLowerRoundedReturningUpper(Vector64<long>   left, Vector64<long>   right);
        public Vector64<uint>   AddLowerRoundedReturningUpper(Vector64<ulong>  left, Vector64<ulong>  right);

        public Vector64<T>  FusedAddHalving(Vector64<T>  left, Vector64<T>  right);
        public Vector128<T> FusedAddHalving(Vector128<T> left, Vector128<T> right);

        public Vector64<T>  FusedAddRoundedHalving(Vector64<T>  left, Vector64<T>  right);
        public Vector128<T> FusedAddRoundedHalving(Vector128<T> left, Vector128<T> right);

        public Vector64<T>  AddSaturate(Vector64<T>  left, Vector64<T>  right);
        public Vector128<T> AddSaturate(Vector128<T> left, Vector128<T> right);

        public Vector64<byte>   SubtractLowerReturningUpper(Vector64<ushort> left, Vector64<ushort> right);
        public Vector64<sbyte>  SubtractLowerReturningUpper(Vector64<short>  left, Vector64<short>  right);
        public Vector64<short>  SubtractLowerReturningUpper(Vector64<int>    left, Vector64<int>    right);
        public Vector64<ushort> SubtractLowerReturningUpper(Vector64<uint>   left, Vector64<uint>   right);
        public Vector64<int>    SubtractLowerReturningUpper(Vector64<long>   left, Vector64<long>   right);
        public Vector64<uint>   SubtractLowerReturningUpper(Vector64<ulong>  left, Vector64<ulong>  right);

        public Vector64<byte>   SubtractLowerRoundedReturningUpper(Vector64<ushort> left, Vector64<ushort> right);
        public Vector64<sbyte>  SubtractLowerRoundedReturningUpper(Vector64<short>  left, Vector64<short>  right);
        public Vector64<short>  SubtractLowerRoundedReturningUpper(Vector64<int>    left, Vector64<int>    right);
        public Vector64<ushort> SubtractLowerRoundedReturningUpper(Vector64<uint>   left, Vector64<uint>   right);
        public Vector64<int>    SubtractLowerRoundedReturningUpper(Vector64<long>   left, Vector64<long>   right);
        public Vector64<uint>   SubtractLowerRoundedReturningUpper(Vector64<ulong>  left, Vector64<ulong>  right);

        public Vector64<T>  FusedSubtractHalving(Vector64<T>  left, Vector64<T>  right);
        public Vector128<T> FusedSubtractHalving(Vector128<T> left, Vector128<T> right);

        public Vector64<T>  SubtractSaturate(Vector64<T>  left, Vector64<T>  right);
        public Vector128<T> SubtractSaturate(Vector128<T> left, Vector128<T> right);

        public partial class Arm64
        {
            public Vector128<byte>   AddUpperReturningUpper(Vector64<byte>   lower, Vector128<ushort> left, Vector128<ushort> right);
            public Vector128<sbyte>  AddUpperReturningUpper(Vector64<sbyte>  lower, Vector128<short>  left, Vector128<short>  right);
            public Vector128<short>  AddUpperReturningUpper(Vector64<short>  lower, Vector128<int>    left, Vector128<int>    right);
            public Vector128<ushort> AddUpperReturningUpper(Vector64<ushort> lower, Vector128<uint>   left, Vector128<uint>   right);
            public Vector128<int>    AddUpperReturningUpper(Vector64<int>    lower, Vector128<long>   left, Vector128<long>   right);
            public Vector128<uint>   AddUpperReturningUpper(Vector64<uint>   lower, Vector128<ulong>  left, Vector128<ulong>  right);

            public Vector128<byte>   AddUpperRoundedReturningUpper(Vector64<byte>   lower, Vector128<ushort> left, Vector128<ushort> right);
            public Vector128<sbyte>  AddUpperRoundedReturningUpper(Vector64<sbyte>  lower, Vector128<short>  left, Vector128<short>  right);
            public Vector128<short>  AddUpperRoundedReturningUpper(Vector64<short>  lower, Vector128<int>    left, Vector128<int>    right);
            public Vector128<ushort> AddUpperRoundedReturningUpper(Vector64<ushort> lower, Vector128<uint>   left, Vector128<uint>   right);
            public Vector128<int>    AddUpperRoundedReturningUpper(Vector64<int>    lower, Vector128<long>   left, Vector128<long>   right);
            public Vector128<uint>   AddUpperRoundedReturningUpper(Vector64<uint>   lower, Vector128<ulong>  left, Vector128<ulong>  right);

            public Vector64<T> AddSaturateScalar(Vector64<T> left, Vector64<T> right);

            public Vector128<byte>   SubtractUpperReturningUpper(Vector64<byte>   lower, Vector128<ushort> left, Vector128<ushort> right);
            public Vector128<sbyte>  SubtractUpperReturningUpper(Vector64<sbyte>  lower, Vector128<short>  left, Vector128<short>  right);
            public Vector128<short>  SubtractUpperReturningUpper(Vector64<short>  lower, Vector128<int>    left, Vector128<int>    right);
            public Vector128<ushort> SubtractUpperReturningUpper(Vector64<ushort> lower, Vector128<uint>   left, Vector128<uint>   right);
            public Vector128<int>    SubtractUpperReturningUpper(Vector64<int>    lower, Vector128<long>   left, Vector128<long>   right);
            public Vector128<uint>   SubtractUpperReturningUpper(Vector64<uint>   lower, Vector128<ulong>  left, Vector128<ulong>  right);

            public Vector128<byte>   SubtractUpperRoundedReturningUpper(Vector64<byte>   lower, Vector128<ushort> left, Vector128<ushort> right);
            public Vector128<sbyte>  SubtractUpperRoundedReturningUpper(Vector64<sbyte>  lower, Vector128<short>  left, Vector128<short>  right);
            public Vector128<short>  SubtractUpperRoundedReturningUpper(Vector64<short>  lower, Vector128<int>    left, Vector128<int>    right);
            public Vector128<ushort> SubtractUpperRoundedReturningUpper(Vector64<ushort> lower, Vector128<uint>   left, Vector128<uint>   right);
            public Vector128<int>    SubtractUpperRoundedReturningUpper(Vector64<int>    lower, Vector128<long>   left, Vector128<long>   right);
            public Vector128<uint>   SubtractUpperRoundedReturningUpper(Vector64<uint>   lower, Vector128<ulong>  left, Vector128<ulong>  right);

            public Vector64<T> SubtractSaturateScalar(Vector64<T> left, Vector64<T> right);

            public static Vector128<short>  AddPairwiseWidening(Vector64<sbyte>  value);
            public static Vector128<ushort> AddPairwiseWidening(Vector64<byte>   value);
            public static Vector128<int>    AddPairwiseWidening(Vector64<short>  value);
            public static Vector128<uint>   AddPairwiseWidening(Vector64<ushort> value);
            public static Vector128<long>   AddPairwiseWidening(Vector64<int>    value);
            public static Vector128<ulong>  AddPairwiseWidening(Vector64<uint>   value);

            public static Vector128<short>  AddPairwiseWideningAndAdd(Vector128<short>  addend, Vector64<sbyte>  value);
            public static Vector128<ushort> AddPairwiseWideningAndAdd(Vector128<ushort> addend, Vector64<byte>   value);
            public static Vector128<int>    AddPairwiseWideningAndAdd(Vector128<int>    addend, Vector64<short>  value);
            public static Vector128<uint>   AddPairwiseWideningAndAdd(Vector128<uint>   addend, Vector64<ushort> value);
            public static Vector128<long>   AddPairwiseWideningAndAdd(Vector128<long>   addend, Vector64<int>    value);
            public static Vector128<ulong>  AddPairwiseWideningAndAdd(Vector128<ulong>  addend, Vector64<uint>   value);

            public static Vector128<ushort> AbsoluteDifferenceWideningLower(Vector128<byte>   left, Vector64<byte>   right);
            public static Vector128<ushort> AbsoluteDifferenceWideningLower(Vector128<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<uint>   AbsoluteDifferenceWideningLower(Vector128<ushort> left, Vector64<ushort> right);
            public static Vector128<uint>   AbsoluteDifferenceWideningLower(Vector128<short>  left, Vector64<short>  right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningLower(Vector128<uint>   left, Vector64<uint>   right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningLower(Vector128<int>    left, Vector64<int>    right);

            public static Vector128<ushort> AbsoluteDifferenceWideningUpper(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> AbsoluteDifferenceWideningUpper(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<uint>   AbsoluteDifferenceWideningUpper(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   AbsoluteDifferenceWideningUpper(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningUpper(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningUpper(Vector128<int>    left, Vector128<int>    right);

            public static Vector128<ushort> AbsoluteDifferenceWideningLowerAndAdd(Vector128<ushort> addend, Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<ushort> AbsoluteDifferenceWideningLowerAndAdd(Vector128<ushort> addend, Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<uint>   AbsoluteDifferenceWideningLowerAndAdd(Vector128<uint>   addend, Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<uint>   AbsoluteDifferenceWideningLowerAndAdd(Vector128<uint>   addend, Vector64<short>  left, Vector64<short>  right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningLowerAndAdd(Vector128<ulong>  addend, Vector64<uint>   left, Vector64<uint>   right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningLowerAndAdd(Vector128<ulong>  addend, Vector64<int>    left, Vector64<int>    right);

            public static Vector128<ushort> AbsoluteDifferenceWideningUpperAndAdd(Vector128<ushort> addend, Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> AbsoluteDifferenceWideningUpperAndAdd(Vector128<ushort> addend, Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<uint>   AbsoluteDifferenceWideningUpperAndAdd(Vector128<uint>   addend, Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   AbsoluteDifferenceWideningUpperAndAdd(Vector128<uint>   addend, Vector128<short>  left, Vector128<short>  right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningUpperAndAdd(Vector128<ulong>  addend, Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  AbsoluteDifferenceWideningUpperAndAdd(Vector128<ulong>  addend, Vector128<int>    left, Vector128<int>    right);

            public static Vector128<short>  AddWideningLower(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<ushort> AddWideningLower(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<int>    AddWideningLower(Vector64<short>  left, Vector64<short>  right);
            public static Vector128<uint>   AddWideningLower(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<long>   AddWideningLower(Vector64<int>    left, Vector64<int>    right);
            public static Vector128<ulong>  AddWideningLower(Vector64<uint>   left, Vector64<uint>   right);

            public static Vector128<short>  AddWideningUpper(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> AddWideningUpper(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<int>    AddWideningUpper(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   AddWideningUpper(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<long>   AddWideningUpper(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<ulong>  AddWideningUpper(Vector128<uint>   left, Vector128<uint>   right);

            public static Vector128<short>  AddWideningLower(Vector128<short>   left, Vector64<sbyte>  right);
            public static Vector128<ushort> AddWideningLower(Vector128<ushort>  left, Vector64<byte>   right);
            public static Vector128<int>    AddWideningLower(Vector128<int>     left, Vector64<short>  right);
            public static Vector128<uint>   AddWideningLower(Vector128<uint>    left, Vector64<ushort> right);
            public static Vector128<long>   AddWideningLower(Vector128<long>    left, Vector64<int>    right);
            public static Vector128<ulong>  AddWideningLower(Vector128<ulong>   left, Vector64<uint>   right);

            public static Vector128<short>  AddWideningUpper(Vector128<short>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> AddWideningUpper(Vector128<ushort> left, Vector128<byte>   right);
            public static Vector128<int>    AddWideningUpper(Vector128<int>    left, Vector128<short>  right);
            public static Vector128<uint>   AddWideningUpper(Vector128<uint>   left, Vector128<ushort> right);
            public static Vector128<long>   AddWideningUpper(Vector128<long>   left, Vector128<int>    right);
            public static Vector128<ulong>  AddWideningUpper(Vector128<ulong>  left, Vector128<uint>   right);

            public static Vector128<short>  SubtractWideningLower(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<ushort> SubtractWideningLower(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<int>    SubtractWideningLower(Vector64<short>  left, Vector64<short>  right);
            public static Vector128<uint>   SubtractWideningLower(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<long>   SubtractWideningLower(Vector64<int>    left, Vector64<int>    right);
            public static Vector128<ulong>  SubtractWideningLower(Vector64<uint>   left, Vector64<uint>   right);

            public static Vector128<short>  SubtractWideningUpper(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> SubtractWideningUpper(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<int>    SubtractWideningUpper(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   SubtractWideningUpper(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<long>   SubtractWideningUpper(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<ulong>  SubtractWideningUpper(Vector128<uint>   left, Vector128<uint>   right);

            public static Vector128<short>  SubtractWideningLower(Vector128<short>   left, Vector64<sbyte>  right);
            public static Vector128<ushort> SubtractWideningLower(Vector128<ushort>  left, Vector64<byte>   right);
            public static Vector128<int>    SubtractWideningLower(Vector128<int>     left, Vector64<short>  right);
            public static Vector128<uint>   SubtractWideningLower(Vector128<uint>    left, Vector64<ushort> right);
            public static Vector128<long>   SubtractWideningLower(Vector128<long>    left, Vector64<int>    right);
            public static Vector128<ulong>  SubtractWideningLower(Vector128<ulong>   left, Vector64<uint>   right);

            public static Vector128<short>  SubtractWideningUpper(Vector128<short>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> SubtractWideningUpper(Vector128<ushort> left, Vector128<byte>   right);
            public static Vector128<int>    SubtractWideningUpper(Vector128<int>    left, Vector128<short>  right);
            public static Vector128<uint>   SubtractWideningUpper(Vector128<uint>   left, Vector128<ushort> right);
            public static Vector128<long>   SubtractWideningUpper(Vector128<long>   left, Vector128<int>    right);
            public static Vector128<ulong>  SubtractWideningUpper(Vector128<ulong>  left, Vector128<uint>   right);

            public static Vector128<short>  MultiplyWideningLower(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningLower(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<int>    MultiplyWideningLower(Vector64<short>  left, Vector64<short>  right);
            public static Vector128<uint>   MultiplyWideningLower(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<long>   MultiplyWideningLower(Vector64<int>    left, Vector64<int>    right);
            public static Vector128<ulong>  MultiplyWideningLower(Vector64<uint>   left, Vector64<uint>   right);

            public static Vector128<short>  MultiplyWideningUpper(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningUpper(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<int>    MultiplyWideningUpper(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   MultiplyWideningUpper(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<long>   MultiplyWideningUpper(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<ulong>  MultiplyWideningUpper(Vector128<uint>   left, Vector128<uint>   right);

            public static Vector128<short>  MultiplyWideningLowerAndAdd(Vector128<short>  addend, Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningLowerAndAdd(Vector128<ushort> addend, Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<int>    MultiplyWideningLowerAndAdd(Vector128<int>    addend, Vector64<short>  left, Vector64<short>  right);
            public static Vector128<uint>   MultiplyWideningLowerAndAdd(Vector128<uint>   addend, Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<long>   MultiplyWideningLowerAndAdd(Vector128<long>   addend, Vector64<int>    left, Vector64<int>    right);
            public static Vector128<ulong>  MultiplyWideningLowerAndAdd(Vector128<ulong>  addend, Vector64<uint>   left, Vector64<uint>   right);

            public static Vector128<short>  MultiplyWideningUpperAndAdd(Vector128<short>  addend, Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningUpperAndAdd(Vector128<ushort> addend, Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<int>    MultiplyWideningUpperAndAdd(Vector128<int>    addend, Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   MultiplyWideningUpperAndAdd(Vector128<uint>   addend, Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<long>   MultiplyWideningUpperAndAdd(Vector128<long>   addend, Vector128<int>    left, Vector128<int>    right);
            public static Vector128<ulong>  MultiplyWideningUpperAndAdd(Vector128<ulong>  addend, Vector128<uint>   left, Vector128<uint>   right);

            public static Vector128<short>  MultiplyWideningLowerAndSubtract(Vector128<short>  minuend, Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningLowerAndSubtract(Vector128<ushort> minuend, Vector64<byte>   left, Vector64<byte>   right);
            public static Vector128<int>    MultiplyWideningLowerAndSubtract(Vector128<int>    minuend, Vector64<short>  left, Vector64<short>  right);
            public static Vector128<uint>   MultiplyWideningLowerAndSubtract(Vector128<uint>   minuend, Vector64<ushort> left, Vector64<ushort> right);
            public static Vector128<long>   MultiplyWideningLowerAndSubtract(Vector128<long>   minuend, Vector64<int>    left, Vector64<int>    right);
            public static Vector128<ulong>  MultiplyWideningLowerAndSubtract(Vector128<ulong>  minuend, Vector64<uint>   left, Vector64<uint>   right);

            public static Vector128<short>  MultiplyWideningUpperAndSubtract(Vector128<short>  minuend, Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> MultiplyWideningUpperAndSubtract(Vector128<ushort> minuend, Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<int>    MultiplyWideningUpperAndSubtract(Vector128<int>    minuend, Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   MultiplyWideningUpperAndSubtract(Vector128<uint>   minuend, Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<long>   MultiplyWideningUpperAndSubtract(Vector128<long>   minuend, Vector128<int>    left, Vector128<int>    right);
            public static Vector128<ulong>  MultiplyWideningUpperAndSubtract(Vector128<ulong>  minuend, Vector128<uint>   left, Vector128<uint>   right);
        }
    }
}
```

## Arm TableVectorLookup and TableVectorExtension intrinsics

**Approved** | [#runtime/1277](https://github.com/dotnet/runtime/issues/1277#issuecomment-600244479)

* We should consider using a custom type rather than a tuple to repesent lists, because we can expose more operations later. It also means people can't do things like taking the address of the tuple field. However, the nice thing about using tuples is language syntax. But we can probably get the same benefits if we were to define implicit conversion between the two. We can also a `Deconstruct` directly on the custom type, allowing for tuple-style deconstructing into locals.
* We don't think we're ready for the overloads that take multiple lists with more than value for .NET 5, due to necessary JIT implementation work
* We approved the `Arm32` version but commented them out b/c we won't be shipping them in .NET 5

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public partial class AdvSimd
    {
        public static Vector64<byte>  VectorTableLookup(Vector128<byte>  table, Vector64<byte>  byteIndexes);
        public static Vector64<sbyte> VectorTableLookup(Vector128<sbyte> table, Vector64<sbyte> byteIndexes);

        public static Vector64<byte>  VectorTableLookupExtension(Vector64<byte> defaultValues, Vector128<byte>  table, Vector64<byte>  byteIndexes);
        public static Vector64<sbyte> VectorTableLookupExtension(Vector64<byte> defaultValues, Vector128<sbyte> table, Vector64<sbyte> byteIndexes);

        // public partial class Arm32
        // {
        //    public static Vector64<byte>  VectorTableLookup(Vector64<byte>  table, Vector64<byte>  byteIndexes);
        //    public static Vector64<sbyte> VectorTableLookup(Vector64<sbyte> table, Vector64<sbyte> byteIndexes);
        //
        //    public static Vector64<byte>  VectorTableLookupExtension(Vector64<byte> defaultValues, Vector64<byte>  table, Vector64<byte> byteIndexes);
        //    public static Vector64<sbyte> VectorTableLookupExtension(Vector64<byte> defaultValues, Vector64<sbyte> table, Vector64<sbyte> byteIndexes);
        // }

        public partial class Arm64
        {
           public static Vector128<byte>  VectorTableLookup(Vector128<byte>  table, Vector128<byte> byteIndexes);
           public static Vector128<sbyte> VectorTableLookup(Vector128<sbyte> table, Vector128<sbyte> byteIndexes);

           public static Vector128<byte>  VectorTableLookupExtension(Vector128<byte> defaultValues, Vector128<byte>  table, Vector128<byte>  byteIndexes);
           public static Vector128<sbyte> VectorTableLookupExtension(Vector128<byte> defaultValues, Vector128<sbyte> table, Vector128<sbyte> byteIndexes);
        }
    }
}
```

