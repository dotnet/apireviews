# Quick Reviews 02/04/2021

## Developers can get immediate feedback on validation problems

**Approved** | [#runtime/36391](https://github.com/dotnet/runtime/issues/36391#issuecomment-773514511) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h0m0s)

* Let's drop the `new()` constraint? The existing `OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations()` method doesn't have it.
* We considered existing types in that namespace but we don't think there is an appropriate home for this.

```C#
namespace Microsoft.Extensions.DependencyInjection
{
    public static class OptionsBuilderValidationExtensions
    {
        public static OptionsBuilder<TOptions> ValidateOnStart<TOptions>(this OptionsBuilder<TOptions> optionsBuilder) where TOptions : class;
    }
}
```

## Don't guard Dictionary<K, V>.Remove(key) by ContainsKey(key)

**Approved** | [#runtime/33797](https://github.com/dotnet/runtime/issues/33797#issuecomment-773517917) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h22m59s)

* Makes sense.

We could further improve the fixer to handle this as well:

```C#
// Before
if (data.ContainsKey(key))
{
    data.Remove(key);
    SomeUnrelatedCode();
}
// After
if (data.Remove(key))
{
    SomeUnrelatedCode();
}
```

But it's not necessary -- we can start simple and take it from there.

## Prefer Dictionary<K, V>.TryGetValue() over guarded indexer access

**Approved** | [#runtime/33798](https://github.com/dotnet/runtime/issues/33798#issuecomment-773519610) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h28m41s)

* Looks good
* This is related to #33797 and #33799. We should make sure these  analyzers don't fight with each other. We might want to put them in the same analyzer and decide which diagnostic is the most appropriate one to raise. They could still generate different IDs.

## Prefer Dictionary<K, V>.TryAddValue(key) over guarded Add(key)

**Approved** | [#runtime/33799](https://github.com/dotnet/runtime/issues/33799#issuecomment-773520822) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h31m43s)

* Looks good
* Please consider the comment [here](https://github.com/dotnet/runtime/issues/33798#issuecomment-773519610).
## Do not call Task.WhenAll with a single argument

**Approved** | [#runtime/33806](https://github.com/dotnet/runtime/issues/33806#issuecomment-773522446) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h33m32s)

* Makes sense, but we shouldn't use `Wait()`:

```C#
async Task M(Task task)
{
    // Before
    await Task.WhenAll(task);

    // After
    await task;
}
```
## Do not call Task.WaitAll with a single argument

**Approved** | [#runtime/33807](https://github.com/dotnet/runtime/issues/33807#issuecomment-773524589) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h36m44s)

* Makes sense

```C#
void M(Task task)
{
    // Before
    Task.WaitAll(task);

    // After
    task.Wait();
}
```

## Use CancellationToken.ThrowIfCancellationRequested

**Approved** | [#runtime/33809](https://github.com/dotnet/runtime/issues/33809#issuecomment-773527472) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h39m30s)

* Looks good
* We should only suggest this for `OperationCanceledException`, not `TaskCanceledException` because `ThrowIfCancellationRequested` throws the former.
## Consider using String.Equals instead of String.Compare

**Approved** | [#runtime/45552](https://github.com/dotnet/runtime/issues/45552) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h44m40s)

## Use String.Contains(char) instead of String.Contains(String)

**Approved** | [#runtime/47180](https://github.com/dotnet/runtime/issues/47180#issuecomment-773532249) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h49m28s)

* Makes sense
* Is this the only overload that we should do this for?
## Obsolete Socket.UseOnlyOverlappedIO

**Approved** | [#runtime/47163](https://github.com/dotnet/runtime/issues/47163#issuecomment-773535418) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h52m42s)

* Looks good
* It feels over-engineered to add a diagnostic ID to this obsoletion

```C#
namespace System.Net.Sockets
{
    public partial class Socket
    {
        [Obsolete]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public bool UseOnlyOverlappedIO { get; set; }
    }
    public partial enum SocketInformationOptions
    {
        [Obsolete]
        [EditorBrowsable(EditorBrowsableState.Never)]
        UseOnlyOverlappedIO = 8,
    }
}
```

## New overload for ServiceController.Stop()

**Approved** | [#runtime/35284](https://github.com/dotnet/runtime/issues/35284#issuecomment-773539215) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=0h58m14s)

* Looks good

```C#
namespace System.ServiceProcess
{
    public partial class ServiceController
    {
        public void Stop(bool stopDependentServices)
    }
}
```

## KeyValuePair<TKey, TValue> should implement IEquatable<T>

**Rejected** | [#runtime/35602](https://github.com/dotnet/runtime/issues/35602#issuecomment-773543902) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=1h4m54s)

* We believe @jkotas concerns outweighs the (minor) benefit of adding this API.
## IEnumerable<T> for XmlNodeList

**Rejected** | [#runtime/38657](https://github.com/dotnet/runtime/issues/38657#issuecomment-773546109) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=1h12m33s)

* This wouldn't help the general `foreach` case due to the pattern-matched `GetEnumerator()` method
* This would only help the Linq case
* It general, it feels like a losing battle to trying to enrich technologies built on non-generic collections implement generic collection interfaces
## Enhance FileSystemEnumerable to track depth recursion

**Approved** | [#runtime/43748](https://github.com/dotnet/runtime/issues/43748#issuecomment-773553629) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=1h16m34s)

* We should `int`
* The default value should be `int.MaxValue`. It seems `0` should just mean don't recurse at all, rather than no limit.

```C#
namespace System.IO
{
    public partial class EnumerationOptions
    {
        public int MaxRecursionDepth { get; set; }
    }
}
```

## string.Join with ReadOnlySpan values parameter

**Rejected** | [#runtime/46255](https://github.com/dotnet/runtime/issues/46255#issuecomment-773560782) | [Video](https://www.youtube.com/watch?v=JltNo-NKTlQ&t=1h30m0s)

* Since one can't stackalloc spans with reference types the value of this API feels low and the existing overload with `IEnumerable<T>` supports Linq-style slicing
* We may want to revisit this based on where the language is going, but for now let's not add this API.
* The scenario is definitely interesting. @GrabYourPitchforks any thoughts?

```C#
namespace System
{
    public partial class String
    {
        public static string Join(char separator, ReadOnlySpan<string?> values);
        public static string Join(string? separator, ReadOnlySpan<string?> values);
    }
}
```

