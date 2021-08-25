# API Review 08/25/2021

## Higher Level SyntaxProvider APIs for incremental generators

**Approved** | [#roslyn/54725](https://github.com/dotnet/roslyn/issues/54725#issuecomment-905876682)

## API Review Notes

Last time, we decided to have AttributeData on the callback. Where to put it?
*    We generally do Contexts here, let's keep doing that

This thing is not a provider, but makes providers. That's a factory, and that's pretty well known. We'll go with SyntaxProviderFactory.

```diff
namespace Microsoft.CodeAnalysis
{
    public readonly struct SyntaxProviderFactory
    {
+       /// <summary>
+       /// Creates an <see cref="IncrementalValuesProvider{T}"/> that can provide a transform over all <see cref="SyntaxNode"/>s marked with a specified attribute.
+       /// </summary>
+       /// <param name="attributeFQN">The FullyQualifiedName of the attribute used to search for nodes that are annotated with it.</param>
+       /// <param name="transform">A function that performs the transfomation of the found nodes</param>
+       /// <returns>An <see cref="IncrementalValuesProvider{T}"/> that provides the results of the transformation</returns>
+       public IncrementalValuesProvider<T> FromAttribute<T>(string attributeFQN, Func<GeneratorAttributeSyntaxContext, AttributeData attributeData, CancellationToken, T> transform);
}

-        public IncrementalValuesProvider<T> CreateSyntaxProvider<T>(Func<SyntaxNode, CancellationToken, bool> predicate, Func<GeneratorSyntaxContext, CancellationToken, T> transform)
+        public IncrementalValuesProvider<T> FromPredicate<T>(Func<SyntaxNode, CancellationToken, bool> predicate, Func<GeneratorSyntaxContext, CancellationToken, T> transform)
    }
+   public readonly struct GeneratorAttributeSyntaxContext
+   {
+       ... Existing properties from GeneratorSyntaxContext
+       public AttributeData AttributeData { get; }
+   }
}
```
## Incremental Generator Work Tracking APIs

**NeedsWork** | [#roslyn/54832](https://github.com/dotnet/roslyn/issues/54832#issuecomment-905880420)

InputSources vs Inputs?
    We'll go with `Inputs`.

Are the IncrementalGeneratorRunStepState names good?
Proposal:
    New
    Modified
    Unchanged
    Cached
    Removed
Name the enum `RunReason` instead?

Output documents have an implicit name
    "Source Output"
    Well-known names go on `IncrementalGeneratorRunStep`
    Could make an analyzer to help ensure no duplication with well-known

Should we just have a property input/output for the start and end of the graph, since they will always be there?
    There are non-source outputs as well. Perhaps `OutputSteps` for all terminal outputs of the graph?
    You _can_ compute it from the `ExecutedSteps`, but that could be confusing

What happens when a name is duplicated?
    You get multiple steps. Generators are separated from each other, so we don't need to worry about generators colliding.

Would `WithTrackingName` make the intention of the method better?
Also `TrackedSteps` instead of `ExecutedSteps`.
If we put `name` on each provider method, it makes naming more discoverable. On the other hand, we don't really want to encourage people to test every intermediate state and name every intermediate step. We should be encouraging just naming a few nodes.

We could add a TimeSpan to the IncrementalGeneratorStep
https://github.com/dotnet/roslyn/pull/55892 is also approved. Use `ElapsedTime` as the name instead.

We should investigate a diff mechanism for the testing SDK

The only remaining open question is around the API design for the `OutputSteps` property, and the naming of the well-known constants. These can be done over email.

```diff
namespace Microsoft.CodeAnalysis
{
    public static class IncrementalValueProviderExtensions
    {
+        public static IncrementalValueProvider<TSource> WithTrackingName<TSource>(this IncrementalValueProvider<TSource> source, string name);
    }
}

namespace Microsoft.CodeAnalysis
{
     public readonly struct GeneratorDriverOptions
     {
        public readonly IncrementalGeneratorOutputKind DisabledOutputs;
+       public readonly bool TrackIncrementalGeneratorSteps;

        public GeneratorDriverOptions(IncrementalGeneratorOutputKind disabledOutputs);
+       public GeneratorDriverOptions(IncrementalGeneratorOutputKind disabledOutputs, bool trackIncrementalGeneratorSteps);
     }

     public class GeneratorRunResult
     {
+       public ImmutableDictionary<string, ImmutableArray<IncrementalGeneratorRunStep>> TrackedSteps { get; }
     }

+    public class IncrementalGeneratorRunStep
+    {
+        ... const strings of the well-known step names
+        public string? Name { get; }
+        public ImmutableArray<(IncrementalGeneratorRunStep Source, int OutputIndex)> Inputs { get; }
+        public ImmutableArray<(object Value, IncrementalGeneratorRunStepState State)> Outputs { get; }
+        public TimeSpan ElapsedTime { get; }
+    }

+    public enum IncrementalGeneratorRunStepState
+    {
+        New,
+        Modified,
+        Unchanged,
+        Cached,
+        Removed
+    }
}
```
## Roslyn should have an ETW/EventPipe source that we can use to report data

**Approved** | [#roslyn/54770](https://github.com/dotnet/roslyn/issues/54770#issuecomment-905881310)

## API Review

Should be `ElapsedTime`, otherwise the API proposed in https://github.com/dotnet/roslyn/pull/55892 looks good.
