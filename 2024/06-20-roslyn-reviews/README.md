# API Review 06/20/2024

## .AsIncrementalGenerator extension method

**Approved** | [#roslyn/74039](https://github.com/dotnet/roslyn/issues/74039#issuecomment-2181510827)

### API Review

* Can the original creator not maintain a reference to the original?
    * The original creator is the analyzer runner itself, we don't have access to that in the tooling.

**Conclusion**: Approved.
## Run a single generator at a time

**Approved** | [#roslyn/74086](https://github.com/dotnet/roslyn/issues/74086#issuecomment-2181511065)

### API Review

* We think a multiple-generator option is the better approach given balanced mode may want to run multiple generators in the future
* We'll change the filter parameter to have a context future-proofing the signature
* We'll also reorder the parameters to put the `Func` second and make it non-optional

```cs
public GeneratorDriver RunGenerators(Compilation compilation, Func<RunGeneratorsFilterContext, bool> generatorFilter, CancellationToken cancellationToken = default);
```

**Conclusion**: Above API shape is approved.
