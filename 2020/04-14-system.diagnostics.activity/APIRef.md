# System.Diagnostics.Activity

```C#
namespace System.Diagnostics
{
    public enum ActivityKind
    {
        Internal = 1,
        Server = 2,
        Client = 3,
        Producer = 4,
        Consumer = 5,
    }
    public readonly struct ActivityContext : IEquatable<ActivityContext>
    {
        public ActivityContext(ActivityTraceId traceId, ActivitySpanId spanId, ActivityTraceFlags traceFlags, string? traceState = null);
        public ActivityTraceId TraceId { get; }
        public ActivitySpanId SpanId { get; }
        public ActivityTraceFlags TraceFlags { get; }
        public string? TraceState { get; }

        public static bool operator ==(ActivityContext context1, ActivityContext context2);
        public static bool operator !=(ActivityContext context1, ActivityContext context2) ;
        public bool Equals(ActivityContext context);
        public override bool Equals(object? obj);
        public override int GetHashCode();
    }
    public sealed class ActivityEvent
    {
        public ActivityEvent(string name);
        public ActivityEvent(string name, DateTimeOffset timestamp);
        public ActivityEvent(string name, IReadOnlyDictionary<string, object> attributes);
        public ActivityEvent(string name, DateTimeOffset timestamp, IReadOnlyDictionary<string, object> attributes);

        public string Name { get; }
        public DateTimeOffset Timestamp { get; }
        public IReadOnlyDictionary<string, object> Attributes { get; }
    }
    public readonly struct ActivityLink : IEquatable<ActivityLink>
    {
        public ActivityLink(ActivityContext context);
        public ActivityLink(ActivityContext context, IDictionary<string, object>? attributes);
        public ActivityContext Context { get; }
        public IDictionary<string, object>? Attributes { get; }

        public override bool Equals(object? obj);
        public bool Equals(ActivityLink link);
        public static bool operator ==(ActivityLink link1, ActivityLink link2);
        public static bool operator !=(ActivityLink link1, ActivityLink link2);
        public override int GetHashCode();
    }
    public enum ActivityDataRequest
    {
        None,
        PropagationData,
        AllData,
        AllDataAndRecorded
    }
    public sealed class ActivitySource : IDisposable
    {
        public ActivitySource(string name, string version);
        public string Name { get; }
        public string Version { get; }
        public bool HasListeners();
        public Activity? StartActivity(string name, ActivityKind kind = ActivityKind.Internal);
        public Activity? StartActivity(
                            string name,
                            ActivityKind kind,
                            ActivityContext parentContext,
                            IEnumerable<KeyValuePair<string, string?>>? tags = null,
                            IEnumerable<ActivityLink>? links = null,
                            DateTimeOffset startTime = default);
        public Activity? StartActivity(
                            string name,
                            ActivityKind kind,
                            string parentId,
                            IEnumerable<KeyValuePair<string, string?>>? tags = null,
                            IEnumerable<ActivityLink>? links = null,
                            DateTimeOffset startTime = default);
        public void Dispose();
        public IDisposable AddActivityListener(
                                System.Func<ActivitySource, bool> listenToSource,
                                Func<
                                    ActivitySource,
                                    string,
                                    ActivityKind,
                                    ActivityContext,
                                    IEnumerable<KeyValuePair<string, string?>>?,
                                    IEnumerable<System.Diagnostics.ActivityLink>?,
                                    ActivityDataRequest> getActivityDataRequestUsingContext,
                                Func<
                                    ActivitySource,
                                    string,
                                    ActivityKind,
                                    ActivityContext,
                                    IEnumerable<KeyValuePair<string, string?>>?,
                                    IEnumerable<System.Diagnostics.ActivityLink>?,
                                    ActivityDataRequest> getActivityDataRequestUsingParentId,
                                Action<Activity> onActivityStarted,
                                Action<Activity> onActivityStopped);
    }
    public partial class Activity : IDisposable
    {
        public ActivityKind Kind { get; } = ActivityKind.Internal;
        public string DisplayName { get; set; }
        public ActivitySource Source { get; }

        public Activity AddEvent(ActivityEvent e);
        public IEnumerable<ActivityEvent> Events { get; }

        public IEnumerable<ActivityLink> Links { get; }

        public ActivityContext Context { get; }

        public bool IsAllDataRequested { get; set;}

        public void SetCustomProperty(string propertyName, object? propertyValue);
        public object? GetCustomProperty(string propertyName);

        public void Dispose();
    }
}
```
