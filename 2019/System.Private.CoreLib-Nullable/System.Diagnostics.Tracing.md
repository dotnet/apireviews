---\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Tracing\before\System.Diagnostics.Tracing.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Tracing\after\System.Diagnostics.Tracing.dll
 namespace System.Diagnostics.Tracing {
  public abstract class DiagnosticCounter : IDisposable {
    public string? DisplayName { get; set; }
    public string? DisplayUnits { get; set; }
    public EventSource EventSource { get; }
    public string Name { get; }
    public void AddMetadata(string key, string value);
    public void Dispose();
  }
  public enum EventActivityOptions {
    Detachable = 8,
    Disable = 2,
    None = 0,
    Recursive = 4,
  }
  public sealed class EventAttribute : Attribute {
    public EventAttribute(int eventId);
    public EventActivityOptions ActivityOptions { get; set; }
    public EventChannel Channel { get; set; }
    public int EventId { get; }
    public EventKeywords Keywords { get; set; }
    public EventLevel Level { get; set; }
    public string? Message { get; set; }
    public EventOpcode Opcode { get; set; }
    public EventTags Tags { get; set; }
    public EventTask Task { get; set; }
    public byte Version { get; set; }
  }
  public enum EventChannel : byte {
    Admin = (byte)16,
    Analytic = (byte)18,
    Debug = (byte)19,
    None = (byte)0,
    Operational = (byte)17,
  }
  public enum EventCommand {
    Disable = -3,
    Enable = -2,
    SendManifest = -1,
    Update = 0,
  }
  public class EventCommandEventArgs : EventArgs {
    public IDictionary<string, string>? Arguments { get; }
    public EventCommand Command { get; }
    public bool DisableEvent(int eventId);
    public bool EnableEvent(int eventId);
  }
  public class EventCounter : DiagnosticCounter {
    public EventCounter(string name, EventSource eventSource);
+   public override string ToString();
    public void WriteMetric(double value);
    public void WriteMetric(float value);
  }
  public class EventDataAttribute : Attribute {
    public EventDataAttribute();
    public string? Name { get; set; }
  }
  public class EventFieldAttribute : Attribute {
    public EventFieldAttribute();
    public EventFieldFormat Format { get; set; }
    public EventFieldTags Tags { get; set; }
  }
  public enum EventFieldFormat {
    Boolean = 3,
    Default = 0,
    Hexadecimal = 4,
    HResult = 15,
    Json = 12,
    String = 2,
    Xml = 11,
  }
  public enum EventFieldTags {
    None = 0,
  }
  public class EventIgnoreAttribute : Attribute {
    public EventIgnoreAttribute();
  }
  public enum EventKeywords : long {
    All = (long)-1,
    AuditFailure = (long)4503599627370496,
    AuditSuccess = (long)9007199254740992,
    CorrelationHint = (long)4503599627370496,
    EventLogClassic = (long)36028797018963968,
    MicrosoftTelemetry = (long)562949953421312,
    None = (long)0,
    Sqm = (long)2251799813685248,
    WdiContext = (long)562949953421312,
    WdiDiagnostic = (long)1125899906842624,
  }
  public enum EventLevel {
    Critical = 1,
    Error = 2,
    Informational = 4,
    LogAlways = 0,
    Verbose = 5,
    Warning = 3,
  }
  public abstract class EventListener : IDisposable {
    protected EventListener();
    public event EventHandler<EventSourceCreatedEventArgs> EventSourceCreated;
    public event EventHandler<EventWrittenEventArgs> EventWritten;
    public void DisableEvents(EventSource eventSource);
    public virtual void Dispose();
    public void EnableEvents(EventSource eventSource, EventLevel level);
    public void EnableEvents(EventSource eventSource, EventLevel level, EventKeywords matchAnyKeyword);
    public void EnableEvents(EventSource eventSource, EventLevel level, EventKeywords matchAnyKeyword, IDictionary<string, string>? arguments);
    protected static int EventSourceIndex(EventSource eventSource);
    protected internal virtual void OnEventSourceCreated(EventSource eventSource);
    protected internal virtual void OnEventWritten(EventWrittenEventArgs eventData);
  }
  public enum EventManifestOptions {
    AllCultures = 2,
    AllowEventSourceOverride = 8,
    None = 0,
    OnlyIfNeededForRegistration = 4,
    Strict = 1,
  }
  public enum EventOpcode {
    DataCollectionStart = 3,
    DataCollectionStop = 4,
    Extension = 5,
    Info = 0,
    Receive = 240,
    Reply = 6,
    Resume = 7,
    Send = 9,
    Start = 1,
    Stop = 2,
    Suspend = 8,
  }
  public class EventSource : IDisposable {
    protected EventSource();
    protected EventSource(bool throwOnEventWriteErrors);
    protected EventSource(EventSourceSettings settings);
    protected EventSource(EventSourceSettings settings, params string[]? traits);
    public EventSource(string eventSourceName);
    public EventSource(string eventSourceName, EventSourceSettings config);
    public EventSource(string eventSourceName, EventSourceSettings config, params string[]? traits);
    public Exception? ConstructionException { get; }
    public static Guid CurrentThreadActivityId { get; }
    public Guid Guid { get; }
    public string Name { get; }
    public EventSourceSettings Settings { get; }
    public event EventHandler<EventCommandEventArgs> EventCommandExecuted;
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    ~EventSource();
    public static string? GenerateManifest(Type eventSourceType, string? assemblyPathToIncludeInManifest);
    public static string? GenerateManifest(Type eventSourceType, string? assemblyPathToIncludeInManifest, EventManifestOptions flags);
    public static Guid GetGuid(Type eventSourceType);
    public static string GetName(Type eventSourceType);
    public static IEnumerable<EventSource> GetSources();
    public string? GetTrait(string key);
    public bool IsEnabled();
    public bool IsEnabled(EventLevel level, EventKeywords keywords);
    public bool IsEnabled(EventLevel level, EventKeywords keywords, EventChannel channel);
    protected virtual void OnEventCommand(EventCommandEventArgs command);
    public static void SendCommand(EventSource eventSource, EventCommand command, IDictionary<string, string>? commandArguments);
    public static void SetCurrentThreadActivityId(Guid activityId);
    public static void SetCurrentThreadActivityId(Guid activityId, out Guid oldActivityThatWillContinue);
    public override string ToString();
    public void Write(string eventName);
    public void Write(string eventName, EventSourceOptions options);
    public void Write<T>(string? eventName, ref EventSourceOptions options, ref Guid activityId, ref Guid relatedActivityId, ref T data);
    public void Write<T>(string? eventName, ref EventSourceOptions options, ref T data);
    public void Write<T>(string? eventName, T data);
    protected void WriteEvent(int eventId);
    protected void WriteEvent(int eventId, byte[]? arg1);
    protected void WriteEvent(int eventId, int arg1);
    protected void WriteEvent(int eventId, int arg1, int arg2);
    protected void WriteEvent(int eventId, int arg1, int arg2, int arg3);
    protected void WriteEvent(int eventId, int arg1, string? arg2);
    protected void WriteEvent(int eventId, long arg1);
    protected void WriteEvent(int eventId, long arg1, byte[]? arg2);
    protected void WriteEvent(int eventId, long arg1, long arg2);
    protected void WriteEvent(int eventId, long arg1, long arg2, long arg3);
    protected void WriteEvent(int eventId, long arg1, string? arg2);
    protected void WriteEvent(int eventId, params object[] args);
    protected void WriteEvent(int eventId, string? arg1);
    protected void WriteEvent(int eventId, string? arg1, int arg2);
    protected void WriteEvent(int eventId, string? arg1, int arg2, int arg3);
    protected void WriteEvent(int eventId, string? arg1, long arg2);
    protected void WriteEvent(int eventId, string? arg1, string? arg2);
    protected void WriteEvent(int eventId, string? arg1, string? arg2, string? arg3);
    protected unsafe void WriteEventCore(int eventId, int eventDataCount, EventSource.EventData* data);
    protected void WriteEventWithRelatedActivityId(int eventId, Guid relatedActivityId, params object[] args);
    protected unsafe void WriteEventWithRelatedActivityIdCore(int eventId, Guid* relatedActivityId, int eventDataCount, EventSource.EventData* data);
    protected internal struct EventData {
      public IntPtr DataPointer { get; set; }
      public int Size { get; set; }
    }
  }
  public sealed class EventSourceAttribute : Attribute {
    public EventSourceAttribute();
    public string? Guid { get; set; }
    public string? LocalizationResources { get; set; }
    public string? Name { get; set; }
  }
  public class EventSourceCreatedEventArgs : EventArgs {
    public EventSourceCreatedEventArgs();
    public EventSource? EventSource { get; }
  }
  public class EventSourceException : Exception {
    public EventSourceException();
    protected EventSourceException(SerializationInfo info, StreamingContext context);
    public EventSourceException(string? message);
    public EventSourceException(string? message, Exception? innerException);
  }
  public struct EventSourceOptions {
    public EventActivityOptions ActivityOptions { get; set; }
    public EventKeywords Keywords { get; set; }
    public EventLevel Level { get; set; }
    public EventOpcode Opcode { get; set; }
    public EventTags Tags { get; set; }
  }
  public enum EventSourceSettings {
    Default = 0,
    EtwManifestEventFormat = 4,
    EtwSelfDescribingEventFormat = 8,
    ThrowOnEventWriteErrors = 1,
  }
  public enum EventTags {
    None = 0,
  }
  public enum EventTask {
    None = 0,
  }
  public class EventWrittenEventArgs : EventArgs {
    public Guid ActivityId { get; }
    public EventChannel Channel { get; }
    public int EventId { get; }
    public string? EventName { get; }
    public EventSource EventSource { get; }
    public EventKeywords Keywords { get; }
    public EventLevel Level { get; }
    public string? Message { get; }
    public EventOpcode Opcode { get; }
    public long OSThreadId { get; }
    public ReadOnlyCollection<object?>? Payload { get; }
    public ReadOnlyCollection<string>? PayloadNames { get; }
    public Guid RelatedActivityId { get; }
    public EventTags Tags { get; }
    public EventTask Task { get; }
    public DateTime TimeStamp { get; }
    public byte Version { get; }
  }
  public class IncrementingEventCounter : DiagnosticCounter {
    public IncrementingEventCounter(string name, EventSource eventSource);
    public TimeSpan DisplayRateTimeScale { get; set; }
    public void Increment(double increment = 1);
+   public override string ToString();
  }
  public class IncrementingPollingCounter : DiagnosticCounter {
    public IncrementingPollingCounter(string name, EventSource eventSource, Func<double> totalValueProvider);
    public TimeSpan DisplayRateTimeScale { get; set; }
+   public override string ToString();
  }
  public sealed class NonEventAttribute : Attribute {
    public NonEventAttribute();
  }
  public class PollingCounter : DiagnosticCounter {
    public PollingCounter(string name, EventSource eventSource, Func<double> metricProvider);
+   public override string ToString();
  }
 }
