# API Review 03/24/2026

## unsafe evolution

**Approved** | [#roslyn/82791](https://github.com/dotnet/roslyn/issues/82791#issuecomment-4120027235)

### API Review

* Rename `RequiresUnsafe` property to `RequiresUnsafeContext`
* We can't remove the other optional-taking API yet, as users will be hit by the ExperimentalAPI warnings. That will need to wait for .NET 11 ship, we will file a bug to follow up.
* Rename `MemorySafetyRules` to `MemorySafetyRulesVersion`
* We will use an enum instead of an `int` for the memory rules version, named as `MemorySafetyRulesVersion`. Values are `Version0`, `Version2`, can be updated when we settle on a final number for the feature.
* `/memorysafetyrules:number` approved as the csc argument.

**Conclusion**: Approved
## Analyzer config option to exclude generated files from BannedApiAnalyzer

**Approved** | [#roslyn/82114](https://github.com/dotnet/roslyn/issues/82114#issuecomment-4119819281)

### API Review

* `dotnet_banned_api_analyzer` for consistency with the public API analyzer options.

**Conclusion**: Approved
