# API Review 05/13/2025

## Add IAsyncDropTarget to allow consuming async capable drops

**Approved** | [#winforms/13422](https://github.com/dotnet/winforms/issues/13422#issuecomment-2877417336) | [Video](https://www.youtube.com/watch?v=-RZ9Shk31h4&t=0h0m0s)

* We asked if "Async" is the right word here, or if it should be something more general like "Background" to convey that it's "not on the UI thread" as opposed to "uses Tasks".  But it doesn't seem to be a significant improvement.
  * It's called "async" in the Win32 API, so calling it something different would inhibit people who are trying to find why their drag/drop isn't working.
* It was asked if any existing controls should be implementing the interface, and the answer was "no", that it was for user opt-in.

```C#
namespace System.Windows.Forms;

public interface IAsyncDropTarget : IDropTarget
{
    void OnAsyncDragDrop(DragEventArgs e);
}
```
## Let's consider to rename `ScreenCaptureMode` into `FormScreenCaptureMode`

**Approved** | [#winforms/13450](https://github.com/dotnet/winforms/issues/13450#issuecomment-2877435823) | [Video](https://www.youtube.com/watch?v=-RZ9Shk31h4&t=0h23m37s)

* Looks good as (re-)proposed.

```C#
namespace System.Windows.Forms;

public partial class Form
{
   public ScreenCaptureMode FormScreenCaptureMode { get; set; }
}

public enum ScreenCaptureMode
{
    Allow = 0,
    HideContent = 1,
    HideWindow = 2,
}
```
## Factory methods to create immutable dictionary instances from dictionary expressions

**Approved** | [#runtime/114090](https://github.com/dotnet/runtime/issues/114090#issuecomment-2877466502) | [Video](https://www.youtube.com/watch?v=-RZ9Shk31h4&t=0h30m18s)

* Looks good as (re-)proposed.

```C#
namespace System.Collections.Immutable
{
    [CollectionBuilder(typeof(ImmutableDictionary), nameof(ImmutableDictionary.CreateRangeWithOverwrite))]
    public sealed class ImmutableDictionary<TKey, TValue> : IDictionary<TKey, TValue> //, ...
    {
        // ...
    }

    public static class ImmutableDictionary
    {
        public static ImmutableDictionary<TKey, TValue> CreateRangeWithOverwrite<TKey, TValue>(
            ReadOnlySpan<KeyValuePair<TKey, TValue>> items)
            where TKey : notnull;

        public static ImmutableDictionary<TKey, TValue> CreateRangeWithOverwrite<TKey, TValue>(
            IEqualityComparer<TKey>? keyComparer,
            ReadOnlySpan<KeyValuePair<TKey, TValue>> items)
            where TKey : notnull;
    }
}

namespace System.Collections.Frozen
{
    [CollectionBuilder(typeof(FrozenDictionary), nameof(FrozenDictionary.Create))]
    public abstract class FrozenDictionary<TKey, TValue> : IDictionary<TKey, TValue> //, ...
    {
        // ...
    }

    public static class FrozenDictionary
    {
        public static FrozenDictionary<TKey, TValue> Create<TKey, TValue>(
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;

        public static FrozenDictionary<TKey, TValue> Create<TKey, TValue>(
            IEqualityComparer<TKey> comparer,
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;
    }
}
```
## Support for generic sender parameter in EventHandler

**Approved** | [#runtime/28289](https://github.com/dotnet/runtime/issues/28289#issuecomment-2877604945) | [Video](https://www.youtube.com/watch?v=-RZ9Shk31h4&t=0h42m6s)

* We added contravariant markers
* We removed the constraint on TSender
* We added allows ref struct
* We applied the same changes to the existing EventHandler<T>

```C#
namespace System
{
    public delegate void EventHandler<in TSender, in TEventArgs>(TSender sender, TEventArgs e)
        where TSender : allows ref struct
        where TEventArgs : allows ref struct;
}
```

```diff
namespace System
{
-   public delegate void EventHandler<TEventArgs>(object sender, TEventArgs e);
+   public delegate void EventHandler<in TEventArgs>(object sender, TEventArgs e)
+       where TEventArgs : allows ref struct;
}
```
## FromServiceKeyAttribute(object) to support nullable (for unkeyed) and to use current service key

**Approved** | [#runtime/113585](https://github.com/dotnet/runtime/issues/113585#issuecomment-2877638542) | [Video](https://www.youtube.com/watch?v=-RZ9Shk31h4&t=1h37m55s)


* NoKey => NullKey, because `null` is the keyword that invokes it.
* Moved InheritKey to the zero value
* The LookupMode property being get-only is correct, it's a calculated value, not a mode selector.

```diff
namespace Microsoft.Extensions.DependencyInjection;

[AttributeUsage(AttributeTargets.Parameter)]
public sealed class FromKeyedServicesAttribute : Attribute
{
-    public FromKeyedServicesAttribute(object key);
    
    // A null key means no key.
+    public FromKeyedServicesAttribute(object? key);

    // Use "inherit key" mode. Key will be null.
+    public FromKeyedServicesAttribute();

-   public object Key { get; }
+   public object? Key { get; }

    // Support a way to determine which of the 3 modes we are in.
+   public ServiceKeyLookupMode LookupMode { get; }
}

// A light-weight alternative is to add a bool "InheritKey" instead of an enum. The Key property would be null when true.
+ public enum ServiceKeyLookupMode
+ {
+    InheritKey,
+    NullKey,
+    ExplicitKey,
+ }
```
