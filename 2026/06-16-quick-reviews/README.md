# API Review 06/16/2026

## IsClosedTypeAttribute: add DerivedTypes property

**Approved** | [#runtime/129009](https://github.com/dotnet/runtime/issues/129009#issuecomment-4721431969) | [Video](https://www.youtube.com/watch?v=ScoIuS-EK_M&t=0h0m0s)

We discussed an optional property vs a ctor parameter, and decided optional property (as proposed) is fine.

```csharp
namespace System.Runtime.CompilerServices
{
    public partial class IsClosedTypeAttribute : Attribute
    {
        public Type[] DerivedTypes { get; set; }
    }
}
```
## Add STJ support for closed hierarchies

**Approved** | [#runtime/129041](https://github.com/dotnet/runtime/issues/129041#issuecomment-4721778669) | [Video](https://www.youtube.com/watch?v=ScoIuS-EK_M&t=0h16m35s)

* Instead of being an attribute on JsonPolymorphicAttribute, we moved it to JsonSerializerOptions and the sourcegen attribute so it can apply to multiple types in one go.

```csharp
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public bool InferClosedTypePolymorphism { get; set; }
    }
}

namespace System.Text.Json.Serialization
{
     public partial class JsonSourceGenerationOptionsAttribute
     {
         public bool InferClosedTypePolymorphism { get; set; }
     }
}
```

## ProcessStartInfo.CreateSuspended, Process.Resume

**Approved** | [#runtime/94127](https://github.com/dotnet/runtime/issues/94127#issuecomment-4721862122) | [Video](https://www.youtube.com/watch?v=ScoIuS-EK_M&t=0h56m40s)

Looks good as proposed

```csharp
namespace System.Diagnostics
{
    public partial class ProcessStartInfo
    {
        public bool StartSuspended { get; set; }
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public partial class SafeProcessHandle
    {
        public void Resume();
    }
}
```
## One-liner for running process with standard handles redirected to null device

**Approved** | [#runtime/128453](https://github.com/dotnet/runtime/issues/128453#issuecomment-4722075301) | [Video](https://www.youtube.com/watch?v=ScoIuS-EK_M&t=1h5m59s)

* ProcessStartInfo can already handle this situation, but we can do it with optional parameters to the Run methods that take a fileName.
* We discussed alternatives for "silent" like "discardOutputs", but kept it as "silent"
* The "removed" methods from this proposal are new in .NET 11 previews, so they are being modified before release rather than just being stripped of their defaults.
  * That makes this a preview->preview breaking change.

```csharp
namespace System.Diagnostics
{
    public class Process : System.ComponentModel.Component, System.IDisposable
    {
-       public static ProcessExitStatus Run(string fileName, IList<string>? arguments = null, TimeSpan? timeout = default);
+       public static ProcessExitStatus Run(string fileName, IList<string>? arguments = null, bool silent = false, TimeSpan? timeout = default);
-       public static Task<ProcessExitStatus> RunAsync(string fileName, IList<string>? arguments = null, CancellationToken cancellationToken = default);
+       public static Task<ProcessExitStatus> RunAsync(string fileName, IList<string>? arguments = null, bool silent = false, CancellationToken cancellationToken = default);
    }
}
```

## Process constructor from SafeProcessHandle

**NeedsWork** | [#runtime/71595](https://github.com/dotnet/runtime/issues/71595#issuecomment-4722402593) | [Video](https://www.youtube.com/watch?v=ScoIuS-EK_M&t=1h29m36s)

We ran out of time before finishing this one.  The most recent proposal version seems well received, just needs some final polish.

* Is there a better namespace (to hide these types)?
* Is ProcessStartInfo needed on the structs?
* Should we add a TState overload?

```csharp
namespace System.Diagnostics;

[SupportedOSPlatformAttribute("windows")]
public ref struct WindowsProcessStartArguments
{
    public WindowsProcessStartArguments();

    public unsafe char* Arguments { get; set; }
    public unsafe char* EnvironmentVariables{ get; set; }

    public ProcessStartInfo ProcessStartInfo { get; set; }
    public nint StandardError { get; set; }
    public nint StandardInput { get; set; }
    public nint StandardOutput { get; set; }

    public static Process Start(ProcessStartInfo startInfo, Func<WindowsProcessStartArguments, 
SafeProcessHandle> callback);
}

[UnsupportedOSPlatformAttribute("windows")]
public ref struct UnixProcessStartArguments
{
    public UnixProcessStartArguments();

    public unsafe byte* ResolvedPath { get; set; }
    public unsafe byte** Arguments { get; set; }
    public unsafe byte** EnvironmentVariables { get; set; }

    public ProcessStartInfo ProcessStartInfo { get; set; }
    public nint StandardError { get; set; }
    public nint StandardInput { get; set; }
    public nint StandardOutput { get; set; }

    public static Process Start(ProcessStartInfo startInfo, Func<UnixProcessStartArguments, SafeProcessHandle> callback);
}
```
