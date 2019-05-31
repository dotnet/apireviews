# Intel Hardware Intrinsics (Round 2)

Status: **Needs more work** |
[Issue](https://github.com/dotnet/corefx/issues/32721) |
[Video](https://www.youtube.com/watch?v=gkQxqUa7KLc)

## Notes

1. To review [the current design of 64-bit only intrinsic](https://github.com/dotnet/coreclr/pull/20146)
   and decide the nested class name (e.g., `X64`, `X64Only`, etc.)

2. To dive into [SSE4.2 STTNI API design](https://github.com/dotnet/coreclr/pull/19958)

    * In the last review, we decided to have 3 enums `StringComparisonMode`,
      `IndexStringComparisonMode`, and `MaskStringComparisonMode`. They have
      some overlapped values and a few unique values to provide "safer"
      semantics for different environments (return bool, indexes, or mask). The
      names of these ComparisonMode match the manual and C/C++ counterparts
      (e.g., `_SIDD_MASKED_NEGATIVE_POLARITY` in C++ for
      `MaskedNegativePolarity` in C#).
    * For intrinsic function names, we decided to encode result flags into more
      understandable function names, for example, `_mm_cmpistra` (C and Z flag
      not set) would be named `CompareNoMatchAndRightNotTerminated` to reflect
      the instruction semantics (too verbose?).

3. To review the redesigned [helper intrinsic APIs in vector classes](https://github.com/dotnet/coreclr/pull/20147).

    * the redesigned helper intrinsic does not provide generic cast intrinsic
      (i.e., `As<T>`) and let users write their own ones via `Unsafe.As<TFrom,
      TTo>`.

Regarding

1. **Approved**. Only concerns was how this will blend with the inheritance
   model, but the corresponding nested type could mirror that.

2. **Needs work**. The suggested ones seem verbose.

3. **Approved**.
    - We should name them `CreateScalarUnsafe` as that follows our pattern.
    - The create method that convers from Vector64/128 to Vector128/256 should
      be `ToVectorXxx`
    - The unsafe version would be `ToVectorXxxUnsafe`
    - The `GetElement`/`SetElement` methods normally should be an indexer, but
      this wouldn't work because the type is immutable.
    - We should add a `As<U>()` instance method