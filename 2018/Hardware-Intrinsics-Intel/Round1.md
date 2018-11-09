# Intel Hardware Intrinsics (Round 1)

Status: **Needs more work** | 
[Presentation](HWIntrinsicAPIReview.pptx) |
[Video](https://www.youtube.com/watch?v=OWwdiStakGI)

## Notes

Below are the notes for the individual areas outlined in the slide.

### SSE 4.2 String Processing Intrinsics

* `StringComparisonMode` seems weird, because some/many values mean different
  things. Can we break them into multiple enums?

### 64-bit only intrinsics (#18744)

* A few methods only work when the processor is in 64-bit mode. If the processor
  is in 32-bit mode, the method will throw.
* We should figure out how we want to deal with. The current design implies
  that if `IsSupported` returns true, all methods will work. Options:
  1. New type
  2. Nested type
  3. Leave, and address via naming, docs, or analyzer.

### Static Cast

* We need to think about how to expose static casts. We can either fully explode
  them with overloads or do partial generics where the input is exploded and
  return is generic.
* These should probably be merged with the hardware-independent vector types as
  they always, they should be methods on those types, like `AsInt64()` etc.
* Why did we choose `Cast` on the unsafe type?

### New intrinsics

* There is a request to expose more scalar intrinsics, like `DivRem`, which we
  can't put on the existing vector based ISA types.
* We considered defining a baseline of what the .NET Core runtimes currently
  support into a type called base, but it's probably easier to have have
  consistency and simple have types for each ISA, like I286, I386 etc.

### Missing intrinsics

* Yeah we should do them :-)
    - [#30266](https://github.com/dotnet/corefx/issues/30266)
      Avx2.BroadcastScalarToVector128(T*) overloads are missing 
    - [#18831](https://github.com/coreclr/corefx/issues/18831) HW intrinsics -
      PopCount() returns int/long, while Leading/TrailingZeroCount() returns
      uint/ulong 
    - [#18145](https://github.com/dotnet/coreclr/issues/18145) Some integer
      scalar and vectored methods/overloads are not exposed in HW intrinsics API
