# API Review 02/29/2024

## Add public API to obtain a location specifier for a call

**NeedsWork** | [#roslyn/72133](https://github.com/dotnet/roslyn/issues/72133#issuecomment-1972075512)

### API Review

* Do we need to put the API on the GeneratorSyntaxContext, or should it go in a more broad location?
* What if the user doesn't know the `hintName` yet in this stage of the pipeline?
* Important scenario for generators is that users can switch on `EmitGeneratedFiles`, then check
  in the files. That won't seem to work here?
    * Compiler provides an option to control where the generated source is emitted
    * Could we just specify from the root of the project?
    * This would potentially also allow us to not need the `hintName` of the generated file, which could simplify the API.
* Could we have `GetInterceptsLocationSpecifier` return the _entire_ globally-qualified attribute?
    * Seems odd that we'd save the user typing and escaping the string, but then still require them to specify the attribute?
    * Users may be trying to make the attribute look prettier, handling imports and the like.
    * Perhaps we may want to have a general "give me a string literal that represents this string value" API
* Can we have "handle" based approach that the compiler has more knowledge of? So that if only the location
  changes, the compiler can understand this, and not rerun everything?
    * Complex topic. Maybe possible, but pretty hard.
    * We may also want to look into a `ForMethodWithName` approach for interceptors which could further optimize this path.
