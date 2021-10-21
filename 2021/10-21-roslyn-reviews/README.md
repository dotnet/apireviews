# API Review 10/21/2021

## IOperation support for interpolated string handler conversions

**Approved** | [#roslyn/54718](https://github.com/dotnet/roslyn/issues/54718#issuecomment-949022790)

##  API Review

The update is generally approved, with the following names for the enum:

```cs
    public enum InterpolatedStringArgumentPlaceholderKind
    {
        CallsiteArgument,
        CallsiteReceiver,
        TrailingValidityArgument,
    }
```
## IOperation support for list patterns

**Approved** | [#roslyn/57194](https://github.com/dotnet/roslyn/issues/57194#issuecomment-949023570)

## API Review

General shape is approved. However, we need to be more specific with the naming, as `Symbol` is not clear and ambiguous. Discussion and approval for naming can take place offline.
## API for IFunctionPointerInvocationOperation

**NeedsWork** | [#roslyn/57195](https://github.com/dotnet/roslyn/issues/57195#issuecomment-949024340)

## API Review

We think there's opportunity for a base interface between `IFunctionPointerInvocationOperation`, `IInvocationOperation`, `IPropertyReferenceOperation`, and `IRaiseEventOperation`. A group will meet offline shortly to design that interface, and we'll revisit this issue with that API in mind.
