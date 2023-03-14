# API Review 03/14/2023

## Add an AppContext switch that disables using reflection by default in JsonSerializerOptions

**Approved** | [#runtime/83279](https://github.com/dotnet/runtime/issues/83279#issuecomment-1468551060) | [Video](https://www.youtube.com/watch?v=KVDnUSH90qI&t=0h0m0s)

* AppCompat switch `System.Text.Json.Serialization.DisableDefaultReflection`
    - We might want to expose an API that we can enlighten the linker about that its return value is tight to this setting.


```diff
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
+   [RequiresUnreferencedCode, RequiresDynamicCode]
    public JsonSerializerOptions(); // default constructor now populates the resolver eagerly by default

+   // new constructor offering linker-safe JsonSerializerOptions construction
+   public JsonSerializerOptions(IJsonTypeInfoResolver? typeInfoResolver); 

    public JsonSerializerOptions(JsonSerializerOptions options); // copy constructor remains linker safe

-   [RequiresUnreferencedCode, RequiresDynamicCode]
    public JsonConverter GetConverter(Type type); // Annotation no longer needed since resolver is not populated lazily
}
```
## API to provide the current system time

**Approved** | [#runtime/36617](https://github.com/dotnet/runtime/issues/36617#issuecomment-1468661805) | [Video](https://www.youtube.com/watch?v=KVDnUSH90qI&t=0h34m27s)

* It seems `TimestampFrequency` should be fixed

```C#
namespace System
{
    public abstract class TimeProvider
    {
        public static TimeProvider System { get; }

        protected TimeProvider(long timestampFrequency);
 
        public abstract DateTimeOffset UtcNow { get; }
        public DateTimeOffset LocalNow { get; }
        public abstract TimeZoneInfo LocalTimeZone { get; }
        public long TimestampFrequency { get; }
  
        public static TimeProvider FromLocalTimeZone(TimeZoneInfo timeZone);
        public abstract long GetTimestamp();
        public TimeSpan GetElapsedTime(long startingTimestamp, long endingTimestamp);
        public abstract ITimer CreateTimer(TimerCallback callback, object? state, TimeSpan dueTime, TimeSpan period);
    }
}
namespace System.Threading
{
    public interface ITimer : IDisposable, IAsyncDisposable
    {
        bool Change(TimeSpan dueTime, TimeSpan period);
    }
}
```

Implementations:

```C#
namespace System.Threading
{
    public partial class Timer : ITimer { }

    public class CancellationTokenSource : IDisposable
    {
        public CancellationTokenSource(TimeSpan delay, TimeProvider timeProvider);
    }
    public sealed class PeriodicTimer : IDisposable
    {
        public PeriodicTimer(TimeSpan period, TimeProvider timeProvider);
    }
}

namespace System.Threading.Tasks
{
    public class Task<TResult> : Task
    {
        public new Task<TResult> WaitAsync(TimeSpan timeout, TimeProvider timeProvider, CancellationToken cancellationToken = default);
    }

    public class Task : IAsyncResult, IDisposable
    {
        public static Task Delay(TimeSpan delay, TimeProvider timeProvider, CancellationToken cancellationToken = default);
        public Task WaitAsync(TimeSpan timeout, TimeProvider timeProvider, CancellationToken cancellationToken = default);
    }
}
```

For downlevel:

```C#
namespace System.Threading.Tasks
{
    public static class TimeProviderTaskExtensions
    {
        public static Task Delay(this TimeProvider timeProvider, TimeSpan delay, CancellationToken cancellationToken = default);
        public static async Task<TResult> WaitAsync<TResult>(this Task<TResult> task, TimeSpan timeout, TimeProvider timeProvider, CancellationToken cancellationToken = default);
        public static async Task WaitAsync(this Task task, TimeSpan timeout, TimeProvider timeProvider, CancellationToken cancellationToken = default);
    }
}
```
