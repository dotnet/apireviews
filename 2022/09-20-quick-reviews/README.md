# API Review 09/20/2022

## Add new overloads to ReadFromJsonAsync method in HttpContentJsonExtensions

**Approved** | [#runtime/72103](https://github.com/dotnet/runtime/issues/72103#issuecomment-1252661506) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=0h0m0s)

* We should add the overloads as presented, with a defaulted CancellationToken; but we need to remove the defaults from JsonSerializerOptions overloads to avoid the defaults making an ambiguous compile error.

```C#
namespace System.Net.Http.Json;

public static partial class HttpContentJsonExtensions
{
        public static Task<object?> ReadFromJsonAsync(this HttpContent content, Type type, CancellationToken cancellationToken = default) { throw null; }
        public static Task<T?> ReadFromJsonAsync<T>(this HttpContent content, CancellationToken cancellationToken = default) { throw null; }

        // Remove the defaults for `JsonSerializerOptions? options`
        public static System.Threading.Tasks.Task<object?> ReadFromJsonAsync(this System.Net.Http.HttpContent content, System.Type type, System.Text.Json.JsonSerializerOptions? options, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; } 
        public static System.Threading.Tasks.Task<T?> ReadFromJsonAsync<T>(this System.Net.Http.HttpContent content, System.Text.Json.JsonSerializerOptions? options, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; } 
 
}
```
## Add snake_case support for System.Text.Json

**Approved** | [#runtime/782](https://github.com/dotnet/runtime/issues/782#issuecomment-1252679781) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=0h10m34s)

* We discussed some other options, like PascalCase (forcing a field or non-conforming name from camelCase to PascalCase)
* We felt that SnakeCaseUpper and friends were better than SnakeUpperCase and friends.

```C#
namespace System.Text.Json;

public partial class JsonNamingPolicy
{
    public static JsonNamingPolicy SnakeCaseLower { get; }
    public static JsonNamingPolicy SnakeCaseUpper { get; }
    public static JsonNamingPolicy KebabCaseLower { get; }
    public static JsonNamingPolicy KebabCaseUpper { get; }
}

public enum JsonKnownNamingPolicy
{
    SnakeCaseLower = 2,
    SnakeCaseUpper = 3,
    KebabCaseLower = 4,
    KebabCaseUpper = 5,
}
```
## MemoryExtensions.Count / number of occurrences of a value in a span

**Approved** | [#runtime/59466](https://github.com/dotnet/runtime/issues/59466#issuecomment-1252693248) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=0h21m42s)

* We asked if we needed to specialize the types in overloading, the answer was no.
* We asked what the answer was for `((ReadOnlySpan<char>)"AAAA").Count("AA")`, and the answer was 2 (non-overlapping count).
  * Please ensure that gets documented and tested.
* There was a hint of a suggestion that we might not need to overload across ROSpan and Span, but we're leaving it as-proposed.

```C#
namespace System
{
    public sealed partial class MemoryExtensions
    {
        public static int Count<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>?;

        public static int Count<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>?;

        public static int Count<T>(this Span<T> span, T value) where T : IEquatable<T>?;

        public static int Count<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>?;
    }
}
```
## Add a DequeueEnqueue method to the PriorityQueue Type

**Approved** | [#runtime/75070](https://github.com/dotnet/runtime/issues/75070#issuecomment-1252702817) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=0h35m0s)

* We're unclear on how useful this method would be, but assuming there's value it looks good as proposed.

```C#
namespace System.Collections.Generic;

public class PriorityQueue<TElement, TPriority>
{
    public TElement DequeueEnqueue(TElement element, TPriority priority);
}
```
## MemoryExtensions.Replace(Span<T>, T, T)

**Approved** | [#runtime/75322](https://github.com/dotnet/runtime/issues/75322#issuecomment-1252730735) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=0h42m51s)

* We discussed whether Replace should also return the replacement count, and the answer was no, because it has a nontrivial cost that exceeds its value (a counting variant could be added in the future, if needed)
  * If someone wants to try experimenting with this and show it's actually negligible, please change it to returning `int`.
* We discussed whether we needed a more complicated name, like ReplaceInPlace, to avoid a confusing overload with future potential replaces.  The feeling was "no".

```C#
namespace System
{
    public static class MemoryExtensions
    {
        public static void Replace<T>(this Span<T> span, T oldValue, T newValue) where T : IEquatable<T>?;
    }
}
```
## Marshaller type to support SafeHandle in source-generated marshalling without hard-coding in the generator

**Approved** | [#runtime/74035](https://github.com/dotnet/runtime/issues/74035#issuecomment-1252755013) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=1h6m36s)

* Looks good as proposed.

```C#
namespace System.Runtime.InteropServices.Marshalling;

[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder), MarshalMode.ManagedToUnmanagedIn, typeof(ManagedToUnmanagedIn))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder), MarshalMode.ManagedToUnmanagedRef, typeof(ManagedToUnmanagedRef))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder), MarshalMode.ManagedToUnmanagedOut, typeof(ManagedToUnmanagedOut))]
public static class SafeHandleMarshaller<T>
    where T : System.Runtime.InteropServices.SafeHandle
{
    public struct ManagedToUnmanagedIn
    {
        public void FromManaged(T handle);

        public IntPtr ToUnmanaged();

        public void Free();
    }

    public struct ManagedToUnmanagedRef
    {
        public ManagedToUnmanagedRef();

        public void FromManaged(T handle);

        public IntPtr ToUnmanaged();

        public void FromUnmanaged(IntPtr value);

        public T ToManagedGuaranteed();

        public void OnInvoked();

        public void Free();
    }

    public struct ManagedToUnmanagedOut
    {
        public ManagedToUnmanagedOut();

        public void FromUnmanaged(IntPtr value);

        public T ToManaged();

        public void OnInvoked();

        public void Free();
    }
}
```
## Add a delegate for OnDirectoryFinished() and others

**NeedsWork** | [#runtime/1953](https://github.com/dotnet/runtime/issues/1953#issuecomment-1252797838) | [Video](https://www.youtube.com/watch?v=kmTMJT4BMtw&t=1h24m48s)

* There's no need for ContinueOnErrorPredicate, it's `Func<int, bool>`
* OnDirectoryFinishedAction should use System.String, not `ReadOnlySpan<char>`.  And then it becomes `Action<string>`.
* We added OnDirectoryStartedAction
* We then removed the prefix "On", but left the suffix "Action" to match the existing "-Predicate" properties.
* We aren't sure if we should remove the "Should" prefix in the on-error handler, or if it's helpful.  

**Action Required:**
* We questioned if the error handler predicate should also take the path that failed.
  * And we didn't know what it should look like (`Func<string, int, bool>`, something complicated with current directory and current candidate as different parameters, etc)


```C#
public class FileSystemEnumerable<TResult> : IEnumerable<TResult>
{
    public Action<string>? DirectoryStartedAction { get; set; } // New API
    public Action<string>? DirectoryFinishedAction { get; set; } // New API

    public Func<int, bool>? ShouldContinueOnErrorPredicate { get; set; } // New API
}
```
