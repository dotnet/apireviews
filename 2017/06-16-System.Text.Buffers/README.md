# API Review for `System.Buffers.Text`

Status: **Needs more work** | [API Reference](System.Text.Buffers.md)

## Feedback

* The assembly name should match the namespace; we have to rename anyway b/c
  they cause collisions between the CoreFxLab's MyGet feed and NuGet.org
* We should consider merging the contents of this assembly with the existing
  assembly
    - This would allow re-implementing the existing encoding APIs to sit on top
      of the new APIs
    - But being out-of-band has some benefit has some benefit, too
* Maybe we should have a more generic name so that we can put all
  transformations in that assembly, instead of just APIs related to text
* `TransformationStatus ` is quite long -- can we make it shorter?
    - `TransformStatus`?
* The `ConvertFrom*` and `Compute*` are too long; makes common call sites quite
  verbose, especially considering the nested classes
* We've some concerns with the name and shape of the API. Are we taking the good
  name here? One proposal is to make the namespace more generic, like
  `System.Buffers.Transformations`
* `ComputeDecodedLength` -> `GetLength`
* `ComputeEncodedBytesFromUtf32` -> `GetLengthFromUtf32`
* `ConvertFromUtf32` -> `FromUtf32`
* Remove `IBufferFormattable` not needed for now
* Why do we need `TextFormat`? Shouldn't we be using the existing
  `NumberFormatInfo`

