# Quick Reviews 6/26/2020

## Guarding calls to platform-specific APIs

**Approved** | [#runtime/33331](https://github.com/dotnet/runtime/issues/33331#issuecomment-650326500) | [Video](https://www.youtube.com/watch?v=R5G4scTRRNQ&t=0h0m0s)

* We should rename `TargetPlatformMinVersion`
    - It's a runtime concept, not a targeting concept
* Can we change `Platform` to `OS`?
* `MinimumPlatform` -> `MinimumOSPlatform`
* `PlatformAttribute` should have an internal constructor
* Add `ObsoletedInPlatformAttribute.Message`
* `RuntimeInformation`
    - `IsOSPlatformOrLater` should also take a `string`
    - `IsOSPlatformEarlierThan` should also take a `string`
    - The `IsPlatformXXx` methods should be recognized by the JIT to be constant
* `OSPlatform`
    - Hide `OSX`
    - Add `macOS`
* Analyzer
    - The analyzer should handle `Debug.Assert` for these methods
    - Should we support helpers methods? We concluded we don't need it yet
## OS compatibility analyzer

**Approved** | [#runtime/37359](https://github.com/dotnet/runtime/issues/37359) | [Video](https://www.youtube.com/watch?v=R5G4scTRRNQ&t=1h21m0s)

