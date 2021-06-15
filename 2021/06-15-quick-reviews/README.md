# API Review 06/15/2021

## Rate limit APIs

**Approved** | [#runtime/52079](https://github.com/dotnet/runtime/issues/52079#issuecomment-861747307) | [Video](https://www.youtube.com/watch?v=bWpUl-P-zo8&t=0h0m0s)


* We decided to rename the QueueProcessingOrder members from { ProcessOldest, ProcessNewest } to { OldestFirst, NewestFirst }
* Instead of having a base `RateLimiterOptions` class, just copy the properties into the derived types.
* The mutability of the options types raised some supportability questions. So we recommend sealing all the options types and removing the property setters.
  * There was further discussion around removing the options types altogether and collapsing them into ctor parameters.
* TokenBucket's settings shouldn't mix the names "permit" and "token".  Unify on "token".
* There was a discussion around the exposed options and the limits to the Acquire method.
  * Since the rate limiters do not expose the PermitLimit (max) value a call to Acquire that exceeds the PermitLimit shouldn't be an ArgumentException (ArgumentExceptions generally mean "caller bug", and in this case the caller couldn't know the input was out of bounds)
    * InvalidOperationException is OK
    * Returning the RateLimitLease saying that it couldn't be acquired is potentially also OK, but there was a suggestion that might lead to infinite/excessive retries on a case that will never succeed.
* TokenBucketLimiter.Replenish isn't authoritative (whether it replenishes or not depends on configuration), but the method name doesn't suggest that.
  * It became `public bool TryReplenish()`
* During discussion the need to pulse/refresh the fixed and sliding window timers came up, which added TryRefresh methods and AutoRefresh configuration properties.

```C#
namespace System.Threading.RateLimiting
{
    // This specifies the behaviour of `WaitAsync` When PermitLimit has been reached
    public enum QueueProcessingOrder
    {
        ProcessOldest,
        ProcessNewest
    }

    public sealed class ConcurrencyLimiterOptions
    {
        public ConcurrencyLimiterOptions(int permitLimit, QueueProcessingOrder queueProcessingOrder, int queueLimit);

        // Specifies the maximum number of permits for the limiter
        public int PermitLimit { get; }
        // Permits exhausted mode, configures `WaitAsync` behaviour
        public QueueProcessingOrder QueueProcessingOrder { get; }
        // Queue limit when queuing is enabled
        public int QueueLimit { get; }
    }

    public sealed class TokenBucketRateLimiterOptions
    {
        public TokenBucketRateLimiterOptions(
            int tokenLimit, 
            QueueProcessingOrder queueProcessingOrder, 
            int queueLimit,
            TimeSpan replenishmentPeriod,
            int tokensPerPeriod,
            bool autoReplenishment = true);

        // Specifies the maximum number of permits for the limiter
        public int TokenLimit { get; }
        // Permits exhausted mode, configures `WaitAsync` behaviour
        public QueueProcessingOrder QueueProcessingOrder { get; }
        // Queue limit when queuing is enabled
        public int QueueLimit { get; }
        // Specifies the period between replenishments
        public TimeSpan ReplenishmentPeriod { get; }
        // Specifies how many tokens to restore each replenishment
        public int TokensPerPeriod { get; }
        // Whether to create a timer to trigger replenishment automatically
        // This parameter is optional
        public bool AutoReplenishment { get; }
    }
    
    // Window based rate limiter options
    public sealed class FixedWindowRateLimiterOptions
    {
        public FixedWindowRateLimiterOptions(
            int permitLimit, 
            QueueProcessingOrder queueProcessingOrder, 
            int queueLimit,
            TimeSpan window,
            bool autoRefresh = true);
        
        // Specifies the maximum number of permits for the limiter
        public int PermitLimit { get; }
        // Permits exhausted mode, configures `WaitAsync` behaviour
        public QueueProcessingOrder QueueProcessingOrder { get; }
        // Queue limit when queuing is enabled
        public int QueueLimit { get; }
        // Specifies the duration of the window where the rate limit is applied
        public TimeSpan Window { get; }

        public bool AutoRefresh { get; }
    }

    public sealed class SlidingWindowRateLimiterOptions
    {
        public SlidingWindowRateLimiterOptions(
            int permitLimit, 
            QueueProcessingOrder queueProcessingOrder, 
            int queueLimit,
            TimeSpan window,
            int segmentsPerWindow,
            bool autoRefresh = true);

        // Specifies the maximum number of permits for the limiter
        public int PermitLimit { get; }
        // Permits exhausted mode, configures `WaitAsync` behaviour
        public QueueProcessingOrder QueueProcessingOrder { get; }
        // Queue limit when queuing is enabled
        public int QueueLimit { get; }
        // Specifies the duration of the window where the rate limit is applied
        public TimeSpan Window { get; }
        // Specifies the number of segments the Window should be divided into
        public int SegmentsPerWindow { get; }
        
        public bool AutoRefresh { get; }
    }

    // Limiter implementations
    public sealed class ConcurrencyLimiter : RateLimiter 
    { 
        public ConcurrencyLimiter(ConcurrencyLimiterOptions options);
        public override int GetAvailablePermits();
        protected override RateLimitLease AcquireCore(int permitCount);
        protected override ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken = default);
    }
    public sealed class TokenBucketRateLimiter : RateLimiter 
    { 
        public FixedWindowRateLimiter(FixedWindowRateLimiterOptions options);
        // Attempts replenish the bucket, returns true if enough time had elapsed and it replenishes; otherwise, false.
        public bool TryReplenish();
        public override int GetAvailablePermits();
        protected override RateLimitLease AcquireCore(int permitCount);
        protected override ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken = default);
    }
    public sealed class FixedWindowRateLimiter : RateLimiter 
    { 
        public FixedWindowRateLimiter(FixedWindowRateLimiterOptions options);
        public bool TryRefresh();
        public override int GetAvailablePermits();
        protected override RateLimitLease AcquireCore(int permitCount);
        protected override ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken = default);
    }
    public sealed class SlidingWindowRateLimiter : RateLimiter 
    { 
        public SlidingWindowRateLimiter(SlidingWindowRateLimiterOptions options);
        public bool TryRefresh();
        public override int GetAvailablePermits();
        protected override RateLimitLease AcquireCore(int permitCount);
        protected override ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken = default);
    }
}
```
## Logging-Generator should allow to skip the IsEnabled check

**Approved** | [#runtime/51927](https://github.com/dotnet/runtime/issues/51927#issuecomment-861750391) | [Video](https://www.youtube.com/watch?v=bWpUl-P-zo8&t=1h17m48s)


Looks good as proposed

```diff
namespace Microsoft.Extensions.Logging
{
    [AttributeUsage(AttributeTargets.Method)]
    public sealed class LoggerMessageAttribute : Attribute
    {
        public LoggerMessageAttribute() { }
        public int EventId { get; set; } = -1;
        public string? EventName { get; set; }
        public LogLevel Level { get; set; } = LogLevel.None;
        public string Message { get; set; } = "";
+       public bool SkipEnabledCheck { get; set; } = false;
    }
}
```
## Obsolete HMACSHA1 constructor with `useManagedSha1`

**Approved** | [#runtime/53875](https://github.com/dotnet/runtime/issues/53875#issuecomment-861751965) | [Video](https://www.youtube.com/watch?v=bWpUl-P-zo8&t=1h22m15s)

Looks good as proposed.

* Use a new ID for HMACSHA1..ctor
* Use a different new ID for all the ProduceLegacyHmacValues properties.
  *  Double check that HMACSHA256 doesn't have one, too.

```diff
namespace System.Security.Cryptography {
    public class HMACSHA1 : HMAC {
+       [Obsolete("HMACSHA1 always uses the algorithm implementation provided by the platform. Use the constructor without the useManagedSha1 parameter.")]
        [EditorBrowsable(EditorBrowsableState.Never)]
        public HMACSHA1(byte[] key, bool useManagedSha1);
    }

    public class HMACSHA384 : HMAC {
+       [Obsolete("Producing legacy HMAC values is no longer supported.")]
        public bool ProduceLegacyHmacValues { get; set; }
    }

    public class HMACSHA512 : HMAC {
+       [Obsolete("Producing legacy HMAC values is no longer supported.")]
        public bool ProduceLegacyHmacValues { get; set; }
    }
}
```
## New Type `System.Runtime.CompilerServices.DependentHandle<TPrimary, TSecondary>`

**Approved** | [#runtime/19459](https://github.com/dotnet/runtime/issues/19459#issuecomment-861769544) | [Video](https://www.youtube.com/watch?v=bWpUl-P-zo8&t=1h24m40s)


* Based on the usage, there's a recommendation to change the `Target` and `Dependent` properties to method pairs to avoid debugger inspection introducing Heisenbugs.
* There are a lot of mixed feelings about the `TargetAndDependent` property.  The most popular opinion is to be `GetTargetAndDependent()`, but "be Decompose" and "be a property" also have their camps.
* There was a suggestion that `Free` should be `Dispose`, but there is pushback on that.
  * "Dispose" should be from IDisposable.
  * IDisposable.Dispose() should support multiple calls without negative side effects.
  * Since the idempotentcy seems unlikely given the raw state of this type, it should not be IDisposable.
* Recommend moving the type to `System.Runtime` since it has nothing / very little to do with compilers, but has to do with garbage collection.  

```C#
namespace System.Runtime
{
    public struct DependentHandle
    {
        public DependentHandle(object? target, object? dependent);

        public bool IsAllocated { get; }

        public object? Target { get; set; }
        public object? Dependent { get; set; }

        (object? Target, object? Dependent) TargetAndDependent { get; }

        public void Free();
    }
}
```
