# API Review 06/02/2026

## New public API for Closed Class Hierarchies feature

**Approved** | [#roslyn/83678](https://github.com/dotnet/roslyn/issues/83678#issuecomment-4605426691)

### API Review

* The simple apis (`SyntaxKind`, `IsClosed`) don't have any feedback given.
* For `TryGetClosedSubtypes`, we don't have prior art for `Try`-based methods here
* How do we want to work with unions as well? Seems likely we'll need it.
* Could we adjust the terminology in the spec to use `case` as well, like unions?
* We want to return a bespoke struct, just in case we need to add another piece of information in the future.

**Conclusion**:

The following API is approved.

```cs
namespace Microsoft.CodeAnalysis
{
    public interface INamedTypeSymbol
    {
+        /// <summary>
+        /// Indicates that the type is restricted from being inherited from outside its containing module.
+        /// </summary>
+        bool IsClosed { get; }

+        ClosedSubtypeInfo GetClosedSubtypes(CancellationToken ct);
    }

+    public readonly struct ClosedSubtypeInfo
+    {
+        public ImmutableArray<INamedTypeSymbol> ClosedSubtypes { get; }
+        public bool IsComplete { get; }
+    }
}

namespace Microsoft.CodeAnalysis.CSharp
{
    public enum SyntaxKind : ushort
    {
+        /// <summary>Represents <see langword="closed"/>.</summary>
+        ClosedKeyword = 8453,
    }
}
```
