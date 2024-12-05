# API Review 12/05/2024

## Add information to `IDeconstructionAssignmentOperation` about the conversions and methods involved when performing the operation.

**Approved** | [#roslyn/74757](https://github.com/dotnet/roslyn/issues/74757#issuecomment-2521491844)

### API Review

* Could we just have the extension method?
    * Wouldn't work well. The pain point is that we have to do language-specifics
* Names look good and match up with existing public API for DeconstructionInfo.
* If we need to, we'll look at calculating for CommonDeconstructionInfo lazily to avoid expensive work.

**Conclusion**: Approved
