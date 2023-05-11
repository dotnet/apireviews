# API Review 05/11/2023

## TcpListener should implement IDisposable

**Approved** | [#runtime/63114](https://github.com/dotnet/runtime/issues/63114#issuecomment-1544434338)

Adding `IDisposable` seems good, but the consensus was to implicitly/publicly implement the interface, even though that means TcpListener will have two equivalent Stop members.

Because the type has already shipped, has existed for a long time, and has very few derived types in the wild, we're okay with saying it doesn't need to bother with the Basic Dispose Pattern (`protected virtual void Dispose(bool)`).

```diff
namespace System.Net.Sockets
{
    public class TcpListener
+        : IDisposable
    {
+         public void Dispose() => Stop();
    }
}
```
## Add ImmutableArray.Contains overload that accepts an IEqualityComparer<T>

**Approved** | [#runtime/66758](https://github.com/dotnet/runtime/issues/66758#issuecomment-1544442675)

Looks good as proposed, except we changed from an extension method to an instance method (as this is an overload of the existing `Contains(T)`).  This seems generally applicable to any `struct` that is `IEnumerable` that provides a `Contains(T)` without a `Contains(T, IEqualityComparer<T>)`... but we couldn't find a second one during the meeting.

```C#
namespace System.Collections.Immutable;

public static partial struct ImmutableArray<T>
{
    public bool Contains(T item, IEqualityComparer<T>? comparer);
}
```
## HttpRequestOptions should implement IReadOnlyDictionary

**Approved** | [#runtime/68149](https://github.com/dotnet/runtime/issues/68149#issuecomment-1544457912)

Looks good as proposed.

We discussed the risks inherent to being both IDictionary and IReadOnlyDictionary (namely, being an ambiguous target for overloading). But we generally think that the risk is low, and the workaround (casting to one of the two interfaces) is easy.

```diff
namespace System.Net.Http
{
    public class HttpRequestOptions :
+        IReadOnlyDictionary<string,object>,
         IDictionary<string, object>
    {
    }
}
```
## Use TimeSpan everywhere we use an int for seconds, milliseconds, and timeouts (Group 2+3)

**Rejected** | [#runtime/67156](https://github.com/dotnet/runtime/issues/67156#issuecomment-1544491429)

The general consensus is that the need for two properties (and the TimeSpan suffix) is probably as much of a detractor to the user experience as the benefit of having clear units for the timeout.

Since this is only for a small number of types, and there's not a whole lot of benefit, we think the answer is to just not make the same mistake in the future and let what we have be what we have.

The individual area owners are highly encouraged to make sure that the `<summary>` text for each of these properties makes the unit very clear.
## add ProcessStartInfo constructor that accepts IEnumerable<string> arguments.

**Approved** | [#runtime/66450](https://github.com/dotnet/runtime/issues/66450#issuecomment-1544500845)

Looks good as proposed.

```C#
namespace System.Diagnostics
{
    public partial class ProcessStartInfo
    {
        public ProcessStartInfo(string fileName, IEnumerable<string> arguments);
    }
}
```
## Make System.Reflection.Emit.ILGenerator constructor protected

**Approved** | [#runtime/86110](https://github.com/dotnet/runtime/issues/86110#issuecomment-1544519252)

Looks good as proposed.

The `protected` constructor and changing the type to `abstract` suggests that some of the `virtual` members should themselves flip to `abstract` (or that there's a `protected abstract` missing from the design).  As a general reminder, `abstract` methods can't be added after the initial release where it becomes possible to actually extend the type, so for ILGenerator that means the current (.NET 8) release.

```diff
namespace System.Reflection.Emit
{
-   public partial class ILGenerator
+   public abstract partial class ILGenerator
    {
-       internal ILGenerator() { }
+       protected ILGenerator() { }
    }
}
```
