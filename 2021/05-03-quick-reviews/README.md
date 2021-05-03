# Quick Reviews 05/03/2021

## Optimize ServiceCollectionDescriptorExtensions TryAdd

**Approved** | [#runtime/44728](https://github.com/dotnet/runtime/issues/44728#issuecomment-831413792) | [Video](https://www.youtube.com/watch?v=FkHK-jcvVRE&t=0h0m0s)

Approved as proposed.

```diff
namespace Microsoft.Extensions.DependencyInjection
{
    public class ServiceCollection
    {
+     public bool TryAdd(ServiceDescriptor descriptor);
+     public bool TryAddEnumerable(ServiceDescriptor descriptor);
+     public bool TryReplace(ServiceDescriptor descriptor);
+     public void RemoveAll(Type serviceType);
    }
}
```
## Change namespace name `System.Text.Json.Node` to `System.Text.Json.Nodes`

**Approved** | [#runtime/51937](https://github.com/dotnet/runtime/issues/51937#issuecomment-831415512) | [Video](https://www.youtube.com/watch?v=FkHK-jcvVRE&t=0h31m40s)

Renaming the namespace is approved as proposed

System.Text.Json.Node => System.Text.Json.Node**s**
## Adding a rough draft of the "minimum viable product" for the .NET Libraries APIs to support generic math

**NeedsWork** | [#designs/205](https://github.com/dotnet/designs/pull/205#issuecomment-831470677) | [Video](https://www.youtube.com/watch?v=FkHK-jcvVRE&t=0h34m42s)

* Should we eliminate IParseable and just add ISpanParseable?
* Rename all lhs parameters to left and all rhs parameters to right
* We need a consistent name for the (low-level) operator defining interfaces.
  * I[Purpose]Operators seems the most reasonable, with Purpose being the primary metadata name, or concept
  * IEquatableOperators => IEqualityOperators
  * IComparableOperators => IComparisonOperators
  * IAddable => IAdditionOperators
  * ISubtractable => ISubtractionOperators
  * IRemainder => IModulusOperators
  * INegatable => IUnaryNegationOperators
  * IShiftable => IShiftOperators
* We need a consistent name for the value-exposing interfaces (MinValue/MaxValue)
  * (Maybe they're the best they're going to be?)
  * IAdditiveIdentity =>
  * IMultiplicativeIdentity =>
  * IMinMaxValue =>
* INumber we discussed, and feel it's probably the right name
* INumber.Create* feels a little weird.  Is it the right level?  Is there a good way to help convey what TOthers do and don't work?
  * Should they be a different interface, higher or lower level?
* BinaryInteger.Rotate* second parameters should be TSelf, not int.
* IBinaryNumber and beyond were only skimmed
* IBinaryNumber's methods probably want some changes (@bartonjs has some regrets about "isUnsigned", at least).
