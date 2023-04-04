# API Review 04/04/2023

## Add an AppContext switch that disables using reflection by default in JsonSerializerOptions

**Approved** | [#runtime/83279](https://github.com/dotnet/runtime/issues/83279#issuecomment-1496340428) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=0h0m0s)

* The AppContext switch will match the fully qualified property name `System.Text.Json.JsonSerializer.IsReflectionEnabledByDefault`
* The switch can be included in `ILLink.Substitutions.xml` to make it a link time constant
* We're probably automatically setting this to `false` when AOT is enabled, at least for console apps and ASP.NET Core
    - However, certain app models like WinForms, WPF, and MAUI will probably still want reflection by default

```C#
namespace System.Text.Json;

public partial static class JsonSerializer
{
    public static bool IsReflectionEnabledByDefault { get; }
}
```

## Make non-file URL support optional in XmlUrlResolver

**Approved** | [#runtime/83495](https://github.com/dotnet/runtime/issues/83495#issuecomment-1496398098) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=0h22m24s)

* Let's do both:
    1. Let's investigate if we can refactor XML so that non-URL based use doesn't result in static dependencies of networking
    2. Let's add a big switch that allows to turn off all networking from XML
* The default behavior
    - It's on by default for now
    - We should look into making it a breaking change to not allow networking by default, but we shouldn't tie it to trimming
    - This would be an extension of what we already did (i.e. not resolving DTDs)
* Name:
    - We prefer `IsNetworkingEnabledByDefault`
    - We should name it like we'd name an API, which means we'd need a type were we'd put it
    - `System.Xml.XmlResolver.IsNetworkingEnabledByDefault`

## HttpClient.Shared

**Approved** | [#runtime/65380](https://github.com/dotnet/runtime/issues/65380#issuecomment-1496418947) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=1h10m11s)

* We should ensure that all properties that allow setting should always be disabled, not just until a request was made.
* We would like to have a behavior like `HttpClient.Shared` functionally behaves like `new HttpClient()` but some settings might be tuned to reflect that it's long lived
* Dispose on the shared instance will be an no-op
* We might want to have a shared property for handlers too, but not instead of this

```C#
namespace System.Net.Http;

public class HttpClient
{
    public static HttpClient Shared { get; }
}
```
## System.Net.HttpStatusCode.UnprocessableContent

**Approved** | [#runtime/70996](https://github.com/dotnet/runtime/issues/70996#issuecomment-1496423509) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=1h26m44s)

* Looks good as proposed
* We decided against updating `Microsoft.AspNetCore.Http.StatusCodes` because it doesn't have any aliases today.

```C#
namespace System.Net;

public enum HttpStatusCode
{
    // UnprocessableEntity = 422
    UnprocessableContent = 422
}
```

## Add CancellationToken to TextWriter.FlushAsync

**Approved** | [#runtime/64340](https://github.com/dotnet/runtime/issues/64340#issuecomment-1496432362) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=1h29m53s)

* Looks good as proposed

```C#
namespace System.IO;

public partial class TextWriter
{
    // Existing:
    // public virtual Task FlushAsync();
    public virtual Task FlushAsync(CancellationToken cancellationToken);
}
```

## Address the TimeProvider community feedback

**Approved** | [#runtime/84274](https://github.com/dotnet/runtime/issues/84274#issuecomment-1496454455) | [Video](https://www.youtube.com/watch?v=nnisgqBZux0&t=1h37m0s)

* Looks good as proposed

```diff
 namespace System;

 public abstract class TimeProvider
 {
     public static TimeProvider System { get; }
     
+    protected TimeProvider();
 
-    public abstract DateTimeOffset UtcNow { get; }
+    public virtual DateTimeOffset GetUtcNow();
 
-    public DateTimeOffset LocalNow { get; }
+    public DateTimeOffset GetLocalNow();
 
-    public abstract TimeZoneInfo LocalTimeZone { get; }
+    public virtual TimeZoneInfo LocalTimeZone { get; }
 
-    public abstract long GetTimestamp();
+    public virtual long GetTimestamp();
 
-    protected TimeProvider(long timestampFrequency);
-    public static TimeProvider FromLocalTimeZone(TimeZoneInfo timeZone);
 
-    public long TimestampFrequency { get; }
+    public virtual long TimestampFrequency { get; }
 
-    public abstract ITimer CreateTimer(TimerCallback callback, object? state, TimeSpan dueTime, TimeSpan period);
+    public virtual ITimer CreateTimer(TimerCallback callback, object? state, TimeSpan dueTime, TimeSpan period);
 
     public TimeSpan GetElapsedTime(long startingTimestamp, long endingTimestamp);
 }
```

