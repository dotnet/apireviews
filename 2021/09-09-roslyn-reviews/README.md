# API Review 09/09/2021

## Make RenameDocumentAction sealed instead of abstract

**Approved** | [#roslyn/55539](https://github.com/dotnet/roslyn/issues/55539#issuecomment-916444827)

## API Review

This is not a breaking change, so this is approved.
## IOperation support for interpolated string handler conversions

**Approved** | [#roslyn/54718](https://github.com/dotnet/roslyn/issues/54718#issuecomment-916444317)

## API Review

We'll use `IInvalidOperation` for bad arguments in the handler creation, rather than `IInterpolatedStringHandlerArgumentPlaceholderOperation`. For the receiver node, `IInterpolatedStringHandlerArgumentPlaceholderOperation.ArgumentIndex` will be -1, and `IsContainingMethodReceiver` will be removed. Otherwise, the API looks good as currently defined.
## Add SymbolVisitor<TArgument, TResult>

**Approved** | [#roslyn/55183](https://github.com/dotnet/roslyn/issues/55183#issuecomment-916446333)

## API Review

We slightly changed the shape of the API to allow for nullability to play better with the visitor result generic. This is inconsistent with the rest of our visitors, but we can look at taking a breaking change in a future API version to allow them to better-express the TResult nullability as well. The amended shape is as follows:

```diff
namespace Microsoft.CodeAnalysis
{
+    public abstract class SymbolVisitor<TArgument, TResult>
+    {
+        protected abstract TResult DefaultResult { get; }
+
+        public virtual TResult Visit(ISymbol? symbol, TArgument argument)
+        {
+            if (symbol != null)
+            {
+                return symbol.Accept(this, argument);
+            }
+
+            return DefaultResult;
+        }
+
+        public virtual TResult DefaultVisit(ISymbol symbol, TArgument argument)
+        {
+            return DefaultResult;
+        }
+
+        public virtual TResult VisitAlias(IAliasSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitArrayType(IArrayTypeSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitAssembly(IAssemblySymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitDiscard(IDiscardSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitDynamicType(IDynamicTypeSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitEvent(IEventSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitField(IFieldSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitLabel(ILabelSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitLocal(ILocalSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitMethod(IMethodSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitModule(IModuleSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitNamedType(INamedTypeSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitNamespace(INamespaceSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitParameter(IParameterSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitPointerType(IPointerTypeSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitFunctionPointerType(IFunctionPointerTypeSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitProperty(IPropertySymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitRangeVariable(IRangeVariableSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+
+        public virtual TResult VisitTypeParameter(ITypeParameterSymbol symbol, TArgument argument)
+        {
+            return DefaultVisit(symbol, argument);
+        }
+    }
```
