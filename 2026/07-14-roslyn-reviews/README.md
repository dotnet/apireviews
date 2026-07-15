# API Review 07/14/2026

## Need an API to get the 'iteration type' of a type.

**Approved** | [#roslyn/77926](https://github.com/dotnet/roslyn/issues/77926#issuecomment-4971889627)

### API Review

* We don't think a flag is the right approach, the API is only relevant to C#.
* Instead, this approach is what we decided on:

```cs
namespace Microsoft.CodeAnalysis
{
    public class SemanticModel
    {
        public ITypeSymbol? GetIterationType(int position, ITypeSymbol type, CancellationToken cancellationToken = default);
    }
}

namespace Microsoft.CodeAnalysis.CSharp
{
    internal class CSharpSemanticModel
    {
         public ITypeSymbol? GetAsyncIterationType(int position, ITypeSymbol type, CancellationToken cancellationToken = default);
    }

    public static class CSharpExtensions
    {
        public ITypeSymbol? GetAsyncIterationType(this SemanticModel, int position, ITypeSymbol type, CancellationToken cancellationToken = default);
    }
}
```

**Conclusion**: Approved
