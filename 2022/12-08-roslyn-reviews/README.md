# API Review 12/08/2022

## Expose constructor that allows for AnalysisOptions and a CancellationToken.

**Approved** | [#roslyn/65051](https://github.com/dotnet/roslyn/pull/65051#issuecomment-1343401737)

## API Review

 * API is approved.
 ```cs
public class CompilationWithAnalyzers
{

        public CompilationWithAnalyzers(
            Compilation compilation,
            ImmutableArray<DiagnosticAnalyzer> analyzers,
            CompilationWithAnalyzersOptions analysisOptions,
            CancellationToken cancellationToken);
}
```
## API to determine if a syntax tree contains *conditional* directives.

**Approved** | [#roslyn/65792](https://github.com/dotnet/roslyn/issues/65792#issuecomment-1343403338)

## API Review

* Should we be able to answer for any types of directive, not just `#if`?
    * Differentiate based on SyntaxKind?
    * Instance method in SyntaxNode taking an int, extension methods for C# and VB taking specific kinds
* Approved with the above change. Final design will be verified over email before merge.
## The `SyntaxGenerator` does not generate implicit conversion operators

**NeedsWork** | [#roslyn/65833](https://github.com/dotnet/roslyn/issues/65833#issuecomment-1343404934)

## API Review

* Should we just make `OperatorDeclaration` handle conversions as well, as conversions are user-defined operators?
* Needs to be further investigated for consistency with existing methods. We'll review again at the next session if there is still any need for public api changes, instead of just implementation changes.
