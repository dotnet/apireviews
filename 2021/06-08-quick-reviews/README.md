# API Review 06/08/2021

## Rate limit APIs

**NeedsWork** | [#runtime/52079](https://github.com/dotnet/runtime/issues/52079#issuecomment-857020857) | [Video](https://www.youtube.com/watch?v=VBY0iuBMqi8&t=0h0m0s)

API Review notes:

* Looking at System.Runtime.* again, we feel that implies something too low level, so we changed back to System.Threading.*
* Now that it's in System.Threading, we believe that System.Threading.RateLimiting rolls off the tongue better than System.Threading.RateLimits.
* All of the extension methods should be non-virtual instance methods, or default parameters
* AvailablePermits needs a verb => GetAvailablePermits
* We felt that the self-documenting behavior of default parameters was better for Acquire and WaitAsync, which justified keeping the template method pattern.
* We did a pretty hefty adjustment to the PermitLease.TryGetMetadata shape to allow the well known keys to indicate what types the metadata value for each key uses.
* We made PermitLease.Dispose follow the Dispose pattern, because that's the pattern.
* We renamed PermitLease to RateLimitLease to give it better affinity to the RateLimiter type.
  * There may be a better name for the permitCount parameters given this rename, but we don't have
* There's a slightly open question as to whether "MetadataName" is overly general, and warrants an affinitized name.


Package name: System.Threading.RateLimiting
Primary namespace: System.Threading.RateLimiting

```C#
namespace System.Threading.RateLimiting
{
    public abstract class RateLimiter
    {
        public abstract int GetAvailablePermits();
        public RateLimitLease Acquire(int permitCount = 1);
        protected abstract RateLimitLease AcquireCore(int permitCount);
        public ValueTask<RateLimitLease> WaitAsync(int permitCount = 1, CancellationToken cancellationToken = default);
        protected abstract ValueTask<RateLimitLease> WaitAsyncCore(int permitCount, CancellationToken cancellationToken);
    }
    public abstract class RateLimitLease : IDisposable
    {
        public abstract bool IsAcquired { get; }

        public abstract bool TryGetMetadata(string metadataName, out object? metadata);
        public bool TryGetMetadata<T>(MetadataName<T> metadataName, [MaybeNullWhen(false)] out T metadata);            

        public abstract IEnumerable<string> MetadataNames { get; }
        public virtual IEnumerable<KeyValuePair<string, object?>> GetAllMetadata();

        public void Dispose() { Dispose(true); GC.SuppressFinalize(this); }
        protected virtual void Dispose(bool disposing);
    }
    public static class MetadataName
    {
        public static MetadataName<TimeSpan> RetryAfter { get; } = Create<TimeSpan>("RETRY_AFTER");
        public static MetadataName<string> ReasonPhrase { get; } = Create<string>("REASON_PHRASE");

        public static MetadataName<T> Create<T>(string name) => new MetadataName<T>(name);
    }
    public sealed class MetadataName<T> : IEquatable<MetadataName<T>>
    {
        public MetadataName(string name);
        public string Name { get; }
    }
}
```
