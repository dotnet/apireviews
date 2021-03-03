# Quick Reviews 02/23/2021

## Analyzer that suggests ToUpperInvariant/ToLowerInvariant in place of ToUpper/ToLower

**Approved** | [#runtime/43976](https://github.com/dotnet/runtime/issues/43976#issuecomment-784413285) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h0m0s)

* This is one of the more noisy analyzers from FxCop
    - It makes sense for class libraries / server projects, but for WinForms, WPF it doesn't really make a difference
    - Can we scope the analyzer for the project type / output type? Not all libraries are the same (e.g. some are user control, some are test projects)
* However, it seems useful to have a severity of hidden so that people can invoke the fixer to replace all calls to the appropriate overload

## Add Tags and Baggage to LogScope using ActivityTrackingOptions

**Approved** | [#runtime/46571](https://github.com/dotnet/runtime/issues/46571#issuecomment-784415533) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h17m43s)

* Looks good as proposed

```C#
namespace Microsoft.Extensions.Logging
{
    public partial enum ActivityTrackingOptions
    {
        Tags        = 0x0020,
        Baggage     = 0x0040,
    }
}
```

## Add back EnumBuilder.CreateType()

**Approved** | [#runtime/46681](https://github.com/dotnet/runtime/issues/46681#issuecomment-784420313) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h21m12s)

* Looks good as proposed
* We should ensure that we address the other issues in the [baseline file](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Reflection.Emit/src/MatchingRefApiCompatBaseline.txt) or add comments indicating why why are by-design.

```C#
namespace System.Reflection.Emit
{
    public sealed partial class EnumBuilder : TypeInfo
    {
        public Type? CreateType();
    }
}
```

## Allow using `SupportedOSPlatformAttribute` on interfaces

**Approved** | [#runtime/47599](https://github.com/dotnet/runtime/issues/47599#issuecomment-784429051) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h29m1s)

* We should also add it to `UnsupportedOSPlatformAttribute`
* Our analyzer should probably warn when types implement the interface but aren't marked as platform-specific for at least the version that the interface is constrained to.
    - If types don't want that, they can suppress the warning and implement the interface methods with guards

```C#
namespace System.Runtime.Versioning
{
    [AttributeUsage(Assembly | Class | Constructor | Enum | Event | Field | Method | Module |
                    Interface /* new */ |
                    Property | Struct, AllowMultiple=true, Inherited=false)]
    public partial class SupportedOSPlatformAttribute
    {
    }

    [AttributeUsage(Assembly | Class | Constructor | Enum | Event | Field | Method | Module |
                    Interface /* new */ |
                    Property | Struct, AllowMultiple=true, Inherited=false)]
    public partial class UnsupportedOSPlatformAttribute 
    {
    }
}
```

## MemoryExtensions.SequenceEqual with IComparer<T>

**Approved** | [#runtime/48304](https://github.com/dotnet/runtime/issues/48304#issuecomment-784438196) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h43m8s)

* Makes sense
* We could drop the constraint on the existing APIs and do the right thing internally, but we can do that later
    - Or we could default the comparer to `null`, which means the caller will either use the existing one when it is equatable and use the new one when it's not.

```C#
namespace System
{
    public static class MemoryExtensions
    {
        // Existing
        // public static bool SequenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T : IEquatable<T>;
        // public static bool SequenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other) where T : System.IEquatable<T>;
        public static bool SequenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, IEqualityComparer<T>? comparer = null);
        public static bool SequenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other, IEqualityComparer<T>? comparer = null);
    }
}
```

## Developers can have access to more options when configuring async awaitables

**Approved** | [#runtime/47525](https://github.com/dotnet/runtime/issues/47525#issuecomment-784477523) | [Video](https://www.youtube.com/watch?v=8kDxa8BfQmw&t=0h57m0s)

* We don't want to do `SuppressExceptions` it has too many issues and it's non-trivial to implement
* `ForceAsync` is niche, but a nice to have
* We could omit `Timeout` and say people who want them can construct a cancellation token but we see people frequently getting this wrong.
* We feel the struct isn't the right path, rather we should just use overloads

```C#
namespace System.Threading.Tasks
{
    public partial class Task
    {
        public ConfiguredCancelableTaskAwaitable ConfigureAwait(TimeSpan timeout);
        public ConfiguredCancelableTaskAwaitable ConfigureAwait(CancellationToken cancellationToken);
        public ConfiguredCancelableTaskAwaitable ConfigureAwait(bool continueOnCapturedContext,
                                                                TimeSpan timeout,
                                                                CancellationToken cancellationToken);

    }
    public partial class Task<TResult>
    {
        public ConfiguredCancelableTaskAwaitable<TResult> ConfigureAwait(TimeSpan timeout);
        public ConfiguredCancelableTaskAwaitable<TResult> ConfigureAwait(CancellationToken cancellationToken);
        public ConfiguredCancelableTaskAwaitable<TResult> ConfigureAwait(bool continueOnCapturedContext,
                                                                         TimeSpan timeout,
                                                                         CancellationToken cancellationToken);

    }
    public partial struct ValueTask
    {
        public ConfiguredCancelableValueTaskAwaitable ConfigureAwait(TimeSpan timeout);
        public ConfiguredCancelableValueTaskAwaitable ConfigureAwait(CancellationToken cancellationToken);
        public ConfiguredCancelableValueTaskAwaitable ConfigureAwait(bool continueOnCapturedContext,
                                                                     TimeSpan timeout,
                                                                     CancellationToken cancellationToken);
    }
    public partial struct ValueTask<TResult>
    {
        public ConfiguredCancelableValueTaskAwaitable<TResult> ConfigureAwait(TimeSpan timeout);
        public ConfiguredCancelableValueTaskAwaitable<TResult> ConfigureAwait(CancellationToken cancellationToken);
        public ConfiguredCancelableValueTaskAwaitable<TResult> ConfigureAwait(bool continueOnCapturedContext,
                                                                              TimeSpan timeout,
                                                                              CancellationToken cancellationToken);

    }
}
namespace System.Runtime.CompilerServices
{
    public readonly struct ConfiguredCancelableTaskAwaitable
    {
        public ConfiguredCancelableTaskAwaiter GetAwaiter();
        public readonly struct ConfiguredCancelableTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public void GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly struct ConfiguredCancelableTaskAwaitable<TResult>
    {
        public ConfiguredCancelableTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public TResult GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly partial struct ConfiguredCancelableValueTaskAwaitable
    {
        public ConfiguredCancelableValueTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public void GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly partial struct ConfiguredCancelableValueTaskAwaitable<TResult>
    {
        public ConfiguredCancelableValueTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public TResult GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
}
```

