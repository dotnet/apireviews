# API Review 06/16/2022

## New public APIs for Checked User-defined Operators

**Approved** | [#roslyn/61846](https://github.com/dotnet/roslyn/issues/61846#issuecomment-1158132498)

### API Review

**Conclusion**: Approved, modulo correcting `chacked` to `checked` in the doc comment.
## New public APIs for Unsigned Right Shift

**Approved** | [#roslyn/61854](https://github.com/dotnet/roslyn/issues/61854#issuecomment-1158133771)

### API Review

**Conclusion**: Approved.
## New public APIs for UTF8 String Literals

**Approved** | [#roslyn/61847](https://github.com/dotnet/roslyn/issues/61847#issuecomment-1158133340)

### API Review

* User-facing documentation should use UTF-8
* Could it be `ILiteralOperation`?
    * It's not a constant, like other literals
    * Couldn't expose the string value from the operation
* Need to sync with the Framework Design folks on the proper spelling of Utf8 in type names (BCL is inconsistent here).

**Conclusion**: Approved, modulo naming of the types
## Decide on rules for WithTrackingName

**Approved** | [#roslyn/61888](https://github.com/dotnet/roslyn/issues/61888#issuecomment-1158134420)

### API Review

**Conclusion**: We should document that tracking names are not considered public API and users cannot depend on them being consistent in generators they don't own.
## IOperation nodes don't expose information about the type parameter that is going to be used as the ".constrained" type for invoking abstract static methods

**Approved** | [#roslyn/53803](https://github.com/dotnet/roslyn/issues/53803#issuecomment-1158134693)

### API Review

**Conclusion**: Approved.
## Update ModuleMetadata to hold onto an object it can keep-alive.

**NeedsWork** | [#roslyn/61778](https://github.com/dotnet/roslyn/issues/61778#issuecomment-1158136463)

### API Review:

* We think that the `owner` and `disposeOwner` parameters are a bit over-general, and that an approach involving passing a callback to be called when the compiler is done with a reference or an event fired at that time would be a better approach.

**Conclusion**: Needs to be reconsidered with the callback approach.
