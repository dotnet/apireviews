# API Review 03/28/2024

## Add public API to obtain a location specifier for a call

**Approved** | [#roslyn/72133](https://github.com/dotnet/roslyn/issues/72133#issuecomment-2026064540)

### API Review

- Why do we need `Version` and `Data`?
    - Because we want to allow users to pretty-print the attribute
- We should document the guarantees around `Data`:
    - Will need no escaping to use as string content
    - Specify the type of string (regular, verbatim, etc)
- Should we make InterceptableLocation an abstract class now so that we can add new internal derived types later?
    - Likely not a breaking change given how roslyn emits method calls, but fine to do anyways.
    - Make the constructor internal or private protected so it can't be inherited publicly
- Move GetInterceptsLocationAttributeSyntax to an extension?
    - Should be fine, it's essentially a helper.

**Conclusion**: Modulo the move to an abstract type and GetInterceptsLocationAttributeSyntax, this is approved.
