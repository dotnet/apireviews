# Quick Reviews 03/09/2021

## Developers can have access to more options when configuring async awaitables

**Approved** | [#runtime/47525](https://github.com/dotnet/runtime/issues/47525#issuecomment-794272772) | [Video](https://www.youtube.com/watch?v=_Svjl1-jauY&t=0h0m0s)

* This API looks cleaner and seems easier to evolve. We don't believe this adds significant overhead of the `ConfigureAwait` approach. In terms of discoverability it will do about the same, because it requires to invoke a method before awaiting the result.
* The existing `Wait` API has overloads that take `int milliseconds` but we consider this a mistake, so we don't replicate them here and just use `TimeSpan`.
* @stephentoub please reopen the issue that allows suppression throwing exceptions because we cut this from this issue.
* Let's cut the `Value` and `ValueTask` ones as there is the additional complexity of accidentally touching it twice and they are really just convenience methods that can be mitigated by calling `AsTask().WaitAsync(...)`

```C#
namespace System.Threading.Tasks
{
    public partial class Task
    {
        public Task WaitAsync(CancellationToken cancellationToken);
        public Task WaitAsync(TimeSpan timeout);
        public Task WaitAsync(TimeSpan timeout, CancellationToken cancellationToken);
    }

    public partial class Task<TResult>
    {
        public Task<TResult> WaitAsync(CancellationToken cancellationToken);
        public Task<TResult> WaitAsync(TimeSpan timeout);
        public Task<TResult> WaitAsync(TimeSpan timeout, CancellationToken cancellationToken);
    }
}
```

## Add API to make configuring the HostOptions easier

**Approved** | [#runtime/48743](https://github.com/dotnet/runtime/issues/48743#issuecomment-794276653) | [Video](https://www.youtube.com/watch?v=_Svjl1-jauY&t=0h24m25s)

* Looks good as proposed

```C#
namespace Microsoft.Extensions.Hosting
{
    public static class HostingHostBuilderExtensions
    {
        public static IHostBuilder ConfigureHostOptions(this IHostBuilder hostBuilder, Action<HostOptions> configure);
    }
}
```

## Add ability to perform zero byte reads in StreamPipeReader

**Approved** | [#runtime/37539](https://github.com/dotnet/runtime/issues/37539#issuecomment-794287642) | [Video](https://www.youtube.com/watch?v=_Svjl1-jauY&t=0h27m46s)

* Last time we said that zero byte reads are technically a violation of the `Stream.Read()` contract, but we believe enough streams support this already that it's better to change the documentation to call this out as something a stream must support. Therefore, we don't need a new API on `Stream`.

```C#
namespace System.IO.Pipelines
{
    public class StreamPipeReaderOptions
    {
        public bool UseZeroByteReads { get; set; }
    }
}
```

## Modern Timer API

**Approved** | [#runtime/31525](https://github.com/dotnet/runtime/issues/31525#issuecomment-794351681) | [Video](https://www.youtube.com/watch?v=_Svjl1-jauY&t=0h36m43s)

* We need to decide which model we want to use when you missed a period (1) should it complete immediately once (2) should it complete immediately for each period that was missed and (3) should it just skip to the next period. Supporting all would require an enum, but we may want to start simple, but we need to agree what "the simple" case is.
    - For ASP.NET's scenarios we only need mode (2).

```C#
namespace System.Threading
{
   public class PeriodicTimer : IDisposable
   {
        public PeriodicTimer(TimeSpan period);
        public ValueTask<bool> WaitForNextTickAsync(CancellationToken cancellationToken = default);
        public void Stop();
   }
}
```

## Add CollectionsMarshal ref accessors for Dict

**Approved** | [#runtime/27062](https://github.com/dotnet/runtime/issues/27062#issuecomment-794381764) | [Video](https://www.youtube.com/watch?v=_Svjl1-jauY&t=1h31m20s)

* Makes sense, we should not use the `Try`-prefix because it refers to the try-pattern which would return a `bool` and out the value

```C#
namespace System.Runtime.InteropServices
{
    public static class CollectionsMarshal
    {        
        public static ref TValue GetValueRef<TKey, TValue>(Dictionary<TKey, TValue> dictionary, [NotNull] TKey key);
        public static ref TValue GetValueRefOrNullRef<TKey, TValue>(Dictionary<TKey, TValue> dictionary, [NotNull] TKey key);
        public static ref TValue GetValueRefOrAddDefault<TKey, TValue>(Dictionary<TKey, TValue> dictionary, [NotNull] TKey key, out bool exists);
    }
}
```

