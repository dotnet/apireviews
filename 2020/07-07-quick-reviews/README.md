# Quick Reviews 7/7/2020

## Improve Activity API usability and OpenTelemetry integration (Part 2)

**Approved** | [#runtime/38419](https://github.com/dotnet/runtime/issues/38419#issuecomment-655054487) | [Video](https://www.youtube.com/watch?v=9sjP8x1L93w&t=0h0m0s)

[Spec](https://github.com/dotnet/designs/pull/134)

* `ActivityContext.IsRemote` sounds like the instance is remoted, which isn't the case, but we can't think of a better name.
* Making `Activity.Kind` settable seems very unfortunate. Given we do this backward compat only, we should find another way of doing this without making one of the key properties mutable. One option is to disallow changing after it was observed (whatever that means). The preferred option would be to find a way that doesn't require mutation (constructor parameter, factory method etc). This part requries more design.
* `Activity.Attributes` seems unfortunate b/c it adds more concepts.
    - Add `SetTag(string, object)`
    - Add `AddTag(string, object)`
    - Add `TagObjects`
    - The existing `Tags` property should probably be marked as obsolete and return all convert all objects to string.
* `ActivityAttributesCollection` should be renamed to something `ActivityTagsCollection`
* `ActivityListener.PregenerateNewRootId` has complex policy so be it

```C#
namespace System.Diagnostics
{
    public readonly partial struct ActivityContext
    {
        // This constructor already exist but we are adding extra defaulted parameter for isRemote.
        public ActivityContext(ActivityTraceId traceId, ActivitySpanId spanId, ActivityTraceFlags traceFlags, string? traceState = null, isRemote = false);

        // This is the new property
        public bool IsRemote { get; }
    }
    public partial class Activity
    {
        public void AddTag(string key, object value);
        public void SetTag(string key, object value);
        public IEnumerable<KeyValuePair<string, object>> TagObjects { get; }
    }
    public readonly struct ActivityEvent
    {
        public ActivityEvent(string name);
        public ActivityEvent(string name, DateTimeOffset? timestamp = null, ActivityTagsCollection? tags = null);
    }
    public readonly partial struct ActivityLink  : IEquatable<ActivityLink>
    {
        public ActivityLink(ActivityContext context, ActivityTagsCollection? tags = null);
    }
    public class ActivityTagsCollection : IDictionary<string, object>
    {
        // Implements this[key] = value as deleting the key
        // Add(key, null) should throw if key already exists, otherwise no-op
        // Should meant that this[key] returns null for-non existing keys too
    }
    public sealed class ActivityListener
    {
        public bool AutoGenerateRootContextTraceId { get; set; }
    }
}
```


## Configuring request options in Browser WebAssembly

**Approved** | [#runtime/34168](https://github.com/dotnet/runtime/issues/34168#issuecomment-655067469) | [Video](https://www.youtube.com/watch?v=9sjP8x1L93w&t=1h44m16s)

* Looks good as proposed

```C#
namespace System.Net.Http
{
    public readonly struct HttpRequestOptionsKey<TValue>
    {
        public HttpRequestOptionsKey(string key);
        public string Key { get; }
    }
    public sealed class HttpRequestOptions : IDictionary<string, object>
    {
        // Explicit interface implementation
        public bool TryGetValue<TValue>(HttpRequestOptionsKey<TValue> key, out TValue value);
        public void Set<TValue>(HttpRequestOptionsKey<TValue> key, TValue value);
    }
    public class HttpRequestMessage : IDisposable
    {
        [Obsolete("Use Options instead.")]
        public IDictionary<string, object> Properties => Options;

        public HttpRequestOptions Options { get; }
    }
}
```

