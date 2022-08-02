# API Review 08/02/2022

## Implement a concept of "Required" properties

**Approved** | [#runtime/29861](https://github.com/dotnet/runtime/issues/29861#issuecomment-1203024852)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization
{
    [AttributeUsage(AttributeTargets.Field | AttributeTargets.Property,
                    AllowMultiple=false)]
    public sealed class JsonRequiredAttribute : JsonAttribute
    {
        public JsonRequiredAttribute();
    }
}

namespace System.Text.Json.Serialization.Metadata
{
    public abstract partial class JsonPropertyInfo
    {
        public bool IsRequired { get; set; }
    }
}
```
## Rename RateLimiter.WaitAsync

**Approved** | [#runtime/71775](https://github.com/dotnet/runtime/issues/71775#issuecomment-1203031411)

* Looks as proposed

```diff
namespace System.Threading.RateLimiting;

public abstract class RateLimiter : IAsyncDisposable, IDisposable
{
-    public RateLimitLease Acquire(int permitCount = 1);
+    public RateLimitLease AttemptAcquire(int permitCount = 1);

-    protected abstract RateLimitLease AcquireCore(int permitCount);
+    protected abstract RateLimitLease AttemptAcquireCore(int permitCount);

-    public ValueTask<RateLimitLease> WaitAndAcquireAsync(int permitCount = 1, CancellationToken cancellationToken = default);
+    public ValueTask<RateLimitLease> AcquireAsync(int permitCount = 1, CancellationToken cancellationToken = default);

-    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(int permitCount, CancellationToken cancellationToken);
+    protected abstract ValueTask<RateLimitLease> AcquireAsyncCore(int permitCount, CancellationToken cancellationToken);
}

public abstract class PartitionedRateLimiter<TResource> : IAsyncDisposable, IDisposable
{
-    public RateLimitLease Acquire(TResource resource, int permitCount = 1);
+    public RateLimitLease AttemptAcquire(TResource resource, int permitCount = 1);

-    protected abstract RateLimitLease AcquireCore(TResource resource, int permitCount);
+    protected abstract RateLimitLease AttemptAcquireCore(TResource resource, int permitCount);

-    public ValueTask<RateLimitLease> WaitAndAcquireAsync(TResource resourceID, int permitCount = 1, CancellationToken cancellationToken = default);
+    public ValueTask<RateLimitLease> AcquireAsync(TResource resourceID, int permitCount = 1, CancellationToken cancellationToken = default);

-    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(TResource resourceID, int permitCount, CancellationToken cancellationToken);
+    protected abstract ValueTask<RateLimitLease> AcquireAsyncCore(TResource resourceID, int permitCount, CancellationToken cancellationToken);
}
```
## Creating an empty temporary directory

**Approved** | [#runtime/72881](https://github.com/dotnet/runtime/issues/72881#issuecomment-1203064309)

* We believe `Path.GetTempFileName()` was a mistake (should have been called `Path.CreateTempFileName()`)
    - Not bad enough to obsolete, but let's at least hide it
* We'd prefer dedicated APIs on `File` and `Directory`
    - We believe customizing the prefix and extension (suffix) for files is desirable.
    - We should disallow path separators for both `prefix` and `suffix`

```C#
namespace System.IO;

public static class Path
{
    // Existing:
    // public static string GetTempPath();
    // public static string GetTempFileName();

    [EditorBrowsable(EditorBrowsableState.Never)]
    public static string GetTempFileName();
}

public partial class File
{
    public static FileInfo CreateTempFile(string? prefix = null, string? suffix = null);
}

public partial class Directory
{
    public static DirectoryInfo CreateTempSubdirectory(string? prefix = null);
}
```
## support System.Diagnostics.TraceSource to be initialized from the app.config file.

**Approved** | [#runtime/72967](https://github.com/dotnet/runtime/issues/72967#issuecomment-1203104819)

* If the goal is to have parity with .NET Framework then we should probably not have more APIs than .NET Framework
    - It seems we need to make some changes because in .NET Framework each subsystem that supported configuration would call into configuration to read data from `app.config` which we don't do in .NET Core because we treat app.config as legacy API that sits on top of everything else.
* This looks this needs another look to see whether we can minimize the API surface
    - By moving the events to the corresponding types we might be able to get rid of the `EventArgs` and just require handlers to cast `sender`
    - Why do we need to expose the defaults? This wasn't necessary on .NET Framework.

Could we make this API shape work?

```C#
namespace System.Diagnostics
{
    public abstract class Switch
    {
-        protected string Value { get; set; }
+        public string Value { get; set; }
+        public void Refresh();
+        public static event EventHandler? Initializing { add; remove; }
    }

    public sealed class Trace
    {
+        public static event EventHandler? Refreshing { add; remove; }
    }

    public class TraceSource
    {
+        public SourceLevels DefaultLevel { get; }
+        public static event EventHandler<ConfigureTraceSourceEventArgs>? Initializing { add; remove; }
    }

+    public sealed class ConfigureTraceSourceEventArgs : EventArgs
+    {
+        public ConfigureTraceSourceEventArgs();
+        public bool WasConfigured { get; set; }
+    }
}
```
## MemoryExtensions.ReferenceEqual

**NeedsWork** | [#runtime/72968](https://github.com/dotnet/runtime/issues/72968#issuecomment-1203132349)

* We don't like `ReferenceEquals` because it's not clear that it covers start and length (as opposed to just start)

```C#
namespace System;

public partial static class MemoryExtensions
{
    public static bool ReferenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other);
    public static bool ReferenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other);
}
```
## Reconsider naming of ObsoletedInOSPlatformAttribute

**Approved** | [#runtime/72970](https://github.com/dotnet/runtime/issues/72970#issuecomment-1203138551)

* We decided to rename it to match other attributes

```diff
 namespace System.Runtime.Versioning;

-public sealed class ObsoletedInOSPlatformAttribute : OSPlatformAttribute
+public sealed class ObsoletedOSPlatformAttribute : OSPlatformAttribute
```
