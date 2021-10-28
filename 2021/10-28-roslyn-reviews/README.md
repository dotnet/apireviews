# API Review 10/28/2021

## API for IFunctionPointerInvocationOperation

**Approved** | [#roslyn/57195](https://github.com/dotnet/roslyn/issues/57195#issuecomment-954299489)

## API Review Notes

After discussion, we don't see much benefit to having a common base type. We don't have any real world scenarios that would benefit much from such a base type, so we will not continue looking at that unless we see more compelling evidence of real scenarios in the area.

After some discussion, we tweaked some names a bit on this API for consistency with the rest of IOperation, and we would also like to add a convenience helper extension method for retrieving the signature of the function pointer type from this node. We don't want to make this a full property on the node, as it's duplicative information, but an extension to make it easier to retrieve will be fine. The final approved shape is:

```diff
namespace Microsoft.CodeAnalysis.Operations
{
+    public interface IFunctionPointerInvocationOperation : IOperation
+    {
+        IOperation Target { get; }
+        ImmutableArray<IArgumentOperation> Arguments { get; }
+    }
    public static class OperationExtensions
    {
+        public IMethodSymbol GetFunctionPointerSignature(this IFunctionPointerInvocationOperation functionPointer);
    }
}

namespace Microsoft.CodeAnalysis
{
    public enum OperationKind
    {
+        FunctionPointerInvocation = 0x72,
    }
}
```
