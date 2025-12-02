# API Review 12/02/2025

## Add EmitDifferenceOptions.DisallowExplicitMethodImplementations

**Approved** | [#roslyn/81305](https://github.com/dotnet/roslyn/issues/81305#issuecomment-3603526178)

### API Review

* Should we remove the negative?
* `MethodImplEntriesSupported` ?

**Conclusion**: Approved with a positive name. Our suggestion is `MethodImplEntriesSupported`.
## Track the project name for a generator driver and report it via ETW

**Approved** | [#roslyn/81036](https://github.com/dotnet/roslyn/pull/81036#issuecomment-3603525149)

### API Review

* We're not sure if `projectName` is sufficient, or appropriate for the compiler layer to handle
    * Does ETW support passing a dictionary
    * Even if it doesn't, there's nothing saying this _needs_ to go to ETW. It could go to some other logging framework.
* What about a dictionary of properties?
    * We don't want a _general_ series of properties, as this is the general generator options. Needs to be specific.
    * `IdentificationProperties`: used by the generator driver to identify itself in logging frameworks.

**Conclusion**: API Approved:

```cs
  -     public GeneratorDriverOptions(IncrementalGeneratorOutputKind disabledOutputs = IncrementalGeneratorOutputKind.None, bool trackIncrementalGeneratorSteps = false, string? baseDirectory = null)
  +     public GeneratorDriverOptions(IncrementalGeneratorOutputKind disabledOutputs = IncrementalGeneratorOutputKind.None, bool trackIncrementalGeneratorSteps = false, string? baseDirectory = null, ImmutableDictionary<string, string?>? identificationProperties = null)
```
