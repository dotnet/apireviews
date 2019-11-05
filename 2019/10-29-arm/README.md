# ARM Intrinsics

Status: **Approved with feedback** | 
[API Ref](System.Runtime.Intrinsics.Arm.md) |
[Video](https://youtu.be/NuggwegnRf0?t=241)

## Notes

* About the same number of intrinsics as for x86
* `ArmBase` is very light, but there is more to expose if we chose to do so
* The name `ArmBase` isn't ideal (we have guidance that says that we shouldn't
  add the `Base`-suffix to base types) but we can't think of a better name.
* `ArmBase.LeadingSignedCount`
    - We should remove the unsigned variants because the operation logically
      only makes sense when you think of the value is signed
    - If it's needed, consumers can still do an unchecked cast
    - If needed we can still add them later
    - However, we acknowledge that `uint` will now call the `long` version, but
      that's also true for `byte` and `ushort`
* `Aes` shouldn't be called for crypto reason, but these are useful building
  blocks for non-crypto cases, like specialized hashes with particular
  distributions
* `Crc32`
    - We can't call the method `Crc32` because that's the type name
    - We should use the prefix `Compute` among all overloads, including the
      nested types
* `Sha1` and `Sha256`
    - The parameter naming is very odd, but the consensus was that since these
      names are special in meaning, we'll keep them in their underscore fashion
* `AdvSimd`
    - `AndNot` might not be the best name, it's different form x86's `AndNot`
      Also, the manual calls it `BitwiseClear`. We believe that makes sense.
    - We decided to not expose `CompareEqualZero` to redeuce bloat. The JIT can
      trivially handle a literal zero
    - We should remove the unsigned version of `LeadingSignCount`
    - `Not` is called `MoveNot`. Should we call it `OnesComplement`? We didn't
      reach a consensus here and determined leaving it as `Not` should be fine
      (especially given the usage of `Not` elsewhere -- e.g. `AndNot`).
* `AdvSimd.Arm64`
    - One of the `CompareXxx` for `Vector64` shuld be a `CompareXxxScalar` 

## Next steps

* Review these items
  - https://github.com/dotnet/corefx/issues/26581
  - https://github.com/dotnet/corefx/issues/42176