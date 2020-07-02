# Quick Reviews 7/2/2020

## Add a new `unmanaged` calling convention bit for use with function pointers

**Approved** | [#runtime/38133](https://github.com/dotnet/runtime/issues/38133#issuecomment-653130834) | [Video](https://www.youtube.com/watch?v=eDDX05uBtFc&t=0h0m0s)

Technically, the actual calling convention is expressed as modopt on the return type but we felt a vague name is good enough.

```C#
namespace System.Reflection.Metadata
{
    public enum SignatureCallingConvention : byte
    {
        Unmanaged = 0x9
    }
}
```

## `System.Runtime.CompilerServices.RuntimeFeature.UnmanagedCallKind`

**Approved** | [#runtime/38135](https://github.com/dotnet/runtime/issues/38135#issuecomment-653135036) | [Video](https://www.youtube.com/watch?v=eDDX05uBtFc&t=0h18m14s)

* We considered keying it off of the presence of the attribute but the attribute isn't the same feature. Also, the compiler would want the type to exist in corlib (to ensure it's not user defined). That would feel odd, considering all modifiers live in `System.Runtime.InteropServices.dll`.
* We agree that making that `RuntimeFeature` a dumping ground for all new compiler features would be bad, but it seems runtime type system constraints would be long to `RuntimeFeature`.

```C#
namespace System.Runtime.CompilerServices
{
    public static partial class RuntimeFeature
    {
        public const string UnmanagedSignatureCallingConvention = nameof(UnmanagedSignatureCallingConvention);
    }
}
```
## Add NativeIntegerAttribute

**Rejected** | [#runtime/38683](https://github.com/dotnet/runtime/issues/38683#issuecomment-653136856) | [Video](https://www.youtube.com/watch?v=eDDX05uBtFc&t=0h27m46s)

* We didn't add `NullableAttribute`. We only added attributes that we expect users to reference explicitly by manually adding custom attributes. It seems this attribute would only be emitted and read by the compiler, so unless there is a reason, we'd not add it.
* Please reopen otherwise.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(
        AttributeTargets.Class |
        AttributeTargets.Event |
        AttributeTargets.Field |
        AttributeTargets.GenericParameter |
        AttributeTargets.Parameter |
        AttributeTargets.Property |
        AttributeTargets.ReturnValue,
        AllowMultiple = false,
        Inherited = false)]
    public sealed class NativeIntegerAttribute : Attribute
    {
        public NativeIntegerAttribute()
        {
            TransformFlags = new[] { true };
        }
        public NativeIntegerAttribute(bool[] flags)
        {
            TransformFlags = flags;
        }
        public IList<bool> TransformFlags { get; }
    }
}
```
## Flexible and efficient optionally-structured console logging out of the box

**Approved** | [#runtime/34742](https://github.com/dotnet/runtime/issues/34742#issuecomment-653193414) | [Video](https://www.youtube.com/watch?v=eDDX05uBtFc&t=0h32m8s)

* Drop `Log` from names
* Rename `Default` so something that doesn't imply a subset relationship, such as `Simple`
* Implementers of `IConsoleFormatter` should use VT100 for colors
* On Windows 7 we still want to show colors, not VT100 escape sequences
* On Windows 10+ we want to see colors, not VT100 escape sequences, even if VT100 processing is off
* Should .NET 5 by default enable VT100 processing? @shirhatti will file an issue for that.

**Assembly:** Microsoft.Extensions.Logging.Abstractions.dll

```diff
 namespace Microsoft.Extensions.Logging
 {
+    public readonly struct LogEntry<TState>
+    {
+       public LogEntry(LogLevel logLevel,
+                       string category,
+                       EventId eventId,
+                       TState state,
+                       Exception exception,
+                       Func<TState, Exception, string> formatter);
+       public LogLevel LogLevel { get; }
+       public string Category { get; }
+       public EventId EventId { get; }
+       public TState State { get; }
+       public Exception Exception { get; }
+       public Func<TState, Exception, string> Formatter { get; }
+    }
}
```

**Assembly:** Microsoft.Extensions.Logging.Console.dll

```diff
 namespace Microsoft.Extensions.Logging
 {
     public static partial class ConsoleLoggerExtensions
     {
         public static ILoggingBuilder AddConsole(this ILoggingBuilder builder);
         public static ILoggingBuilder AddConsole(this ILoggingBuilder builder, Action<ConsoleLoggerOptions> configure);
+        public static ILoggingBuilder AddConsoleFormatter<TFormatter, TOptions>(this ILoggingBuilder builder)
+                                      where TFormatter : class, IConsoleFormatter where TOptions : ConsoleFormatterOptions;
+        public static ILoggingBuilder AddConsoleFormatter<TFormatter, TOptions>(this ILoggingBuilder builder, Action<TOptions> configure)
+                                      where TFormatter : class, IConsoleFormatter where TOptions : ConsoleFormatterOptions;
+        public static ILoggingBuilder AddSimpleConsole(this ILoggingBuilder builder, Action<SimpleConsoleFormatterOptions> configure);
+        public static ILoggingBuilder AddJsonConsole(this ILoggingBuilder builder, Action<JsonConsoleFormatterOptions> configure);
+        public static ILoggingBuilder AddSystemdConsole(this ILoggingBuilder builder, Action<ConsoleFormatterOptions> configure);
     }
 }
 namespace Microsoft.Extensions.Logging.Console
 {
+    public static partial class ConsoleFormatterNames
+    {
+        public const string Simple = "simple";
+        public const string Json = "json";
+        public const string Systemd = "systemd";
+    }
+    [ObsoleteAttribute("ConsoleLoggerFormat has been deprecated.", false)]
     public enum ConsoleLoggerFormat
     {
         Default = 0,
         Systemd = 1,
     }
     public partial class ConsoleLoggerOptions
     {
         public ConsoleLoggerOptions();
+        [ObsoleteAttribute("ConsoleLoggerOptions.DisableColors has been deprecated. Please use ColoredConsoleFormatterOptions.DisableColors instead.", false)]
         public bool DisableColors { get; set; }
+        [ObsoleteAttribute("ConsoleLoggerOptions.Format has been deprecated. Please use ConsoleLoggerOptions.FormatterName instead.", false)]
         public ConsoleLoggerFormat Format { get; set; }
         public string FormatterName { get; set; }
+        [ObsoleteAttribute("ConsoleLoggerOptions.IncludeScopes has been deprecated..", false)]
         public bool IncludeScopes { get; set; }
         public LogLevel LogToStandardErrorThreshold { get; set; }
+        [ObsoleteAttribute("ConsoleLoggerOptions.TimestampFormat has been deprecated..", false)]
         public string TimestampFormat { get; set; }
+        [ObsoleteAttribute("ConsoleLoggerOptions.UseUtcTimestamp has been deprecated..", false)]
         public bool UseUtcTimestamp { get; set; }
     }
     [ProviderAliasAttribute("Console")]
     public partial class ConsoleLoggerProvider : ILoggerProvider, ISupportExternalScope, IDisposable
     {
         public ConsoleLoggerProvider(IOptionsMonitor<ConsoleLoggerOptions> options);
+        public ConsoleLoggerProvider(IOptionsMonitor<ConsoleLoggerOptions> options, IEnumerable<IConsoleFormatter> formatters);
         public ILogger CreateLogger(string name);
         public void Dispose();
         public void SetScopeProvider(IExternalScopeProvider scopeProvider);
     }
+    public partial class SimpleConsoleFormatterOptions : ConsoleFormatterOptions
+    {
+        public SimpleConsoleFormatterOptions();
+        public bool DisableColors { get; set; }
+        public bool SingleLine { get; set; }
+    }
+    public partial abstract ConsoleFormatter
+    {
+        protected ConsoleFormatter(string name);
+        public string Name { get; }
+        public abstract void Write<TState>(in LogEntry<TState> logEntry,
+                                           IExternalScopeProvider scopeProvider,
+                                           TextWriter textWriter);
+    }
+    public partial class JsonConsoleFormatterOptions : ConsoleFormatterOptions
+    {
+        public JsonConsoleFormatterOptions();
+        public JsonWriterOptions JsonWriterOptions { get; set; }
+    }
+    public partial class ConsoleFormatterOptions
+    {
+        public ConsoleFormatterOptions();
+        public bool IncludeScopes { get; set; }
+        public string TimestampFormat { get; set; }
+        public bool UseUtcTimestamp { get; set; }
+    }
}
```

## HTTP2: Create additional connections when maximum active streams is reached

**Approved** | [#runtime/34742](https://github.com/dotnet/runtime/issues/35088#issuecomment-653248184) | [Video](https://www.youtube.com/watch?v=7XhZsgJTywg&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Net.Http
{
    public partial class SocketsHttpHandler : HttpMessageHandler
    {
        public int MaxHttp2ConnectionsPerServer { get; set; }
    }   
    public partial class WinHttpHandler : HttpMessageHandler
    {
        public bool EnableMultipleHttp2Connections { get; set; }
    }
}
```


