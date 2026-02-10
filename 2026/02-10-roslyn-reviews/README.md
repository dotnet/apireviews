# API Review 02/10/2026

## AnalyzerConfigSet.GetOptionsForSourcePath support for [$generated$] sections

**NeedsWork** | [#roslyn/82285](https://github.com/dotnet/roslyn/issues/82285#issuecomment-3880814116)

### API Review

* Let's do something specific for generators
* Have the backcompat merging logic live in `AnalyzerConfigOptionsSet`
* Do we need to pass through a new option for the project directory through the targets?
    * Yes, we will need this. No existing flag covers this completely.
* Semantics: the full path to a generated file is `<path-to-project-directory>/$generated$/<generator-name>/hintname`
* API shape: `public AnalyzerConfigOptionsResult GetOptionsForGeneratedPath(string projectBaseDirectory, string projectOutputDirectory, string generatedFileOutputPath)`

**Conclusion**: We will move forward with this shape, and follow up over email with the final changes for the msbuild task and compiler options necessary to plumb `projectBaseDirectory` through.
## Analyzer config option to exclude generated files from BannedApiAnalyzer

**Rejected** | [#roslyn/82114](https://github.com/dotnet/roslyn/issues/82114#issuecomment-3880818612)

We've rejected this in favor of https://github.com/dotnet/roslyn/issues/82285, and allowing the user to specify options via a new `.editorconfig` section.
