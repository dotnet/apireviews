# API Review 02/01/2024

## Add `IParameterSymbol.IsParamCollection` property 

**Approved** | [#roslyn/71816](https://github.com/dotnet/roslyn/issues/71816#issuecomment-1922332863)

### API Review

* Only C# will recognize these extra types as `params` parameters
* Naming with an `s`?
    * We've gone with C# naming for these in the past
* Could consider an enum option if we think there could be more `params` things in the future?
    * Considered for completeness, not particularly caring
* IDE usage seems like it would be fine with option 3, and may be the easiest to absorb.
    * There's at least one case in the IDE codebase that does a hard cast
    * `params` can be put on bad things in source, would analyzers have to handle this already?
        * The cast will now break on _correct_ code, which it would not have before
* Going through references in grep.app, nearly all would be better served by option 3. Only one example of obviously bad code in the presence of option 3.
    * Given this, we prefer option 3

**Conclusion**: We will go with option 3, with `IsParamsArray` and `IsParamsCollection` as the property names.
