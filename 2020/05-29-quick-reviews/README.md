# Quick Reviews 5/29/2020

## RequireMethodImplToRemainInEffectAttribute

**Approved** | [#runtime/35315](https://github.com/dotnet/runtime/issues/35315#issuecomment-636093159) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=0h0m0s)

Let's go with PreserveBaseOverridesAttribute then.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
    public sealed class PreserveBaseOverridesAttribute : Attribute
    {
    }
}
```
## Use of `EditorBrowsableState.Never` leads language service to claim it's gone

**Approved** | [#runtime/720](https://github.com/dotnet/runtime/issues/720#issuecomment-636098716) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=0h22m27s)

```diff
 public enum ComInterfaceType 
 { 
-    [System.ComponentModel.EditorBrowsableAttribute(System.ComponentModel.EditorBrowsableState.Never)] 
-    [System.ObsoleteAttribute("Support for IDispatch may be unavailable in future releases.")] 
     InterfaceIsDual = 0, 
     InterfaceIsIUnknown = 1, 
-    [System.ComponentModel.EditorBrowsableAttribute(System.ComponentModel.EditorBrowsableState.Never)] 
-    [System.ObsoleteAttribute("Support for IDispatch may be unavailable in future releases.")] 
     InterfaceIsIDispatch = 2, 
     InterfaceIsIInspectable = 3, 
 }
```
## New API: ModuleInitializerAttribute

**Approved** | [#runtime/35749](https://github.com/dotnet/runtime/issues/35749#issuecomment-636105051) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=0h35m8s)

@jaredpar presumably this feature won't have language syntax. Normally we don't use `S.R.CompilerServices` for stuff that developers are expected to use directly. Normally, the namespace is for stuff the compiler generates calls for. However, in this case it seems on par with other very advanced features that we don't want the general public to use, this feels OK.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]
    public sealed class ModuleInitializerAttribute : Attribute
    {
    }
}
```
## Allow DynamicallyAccessedMembersAttribute on methods

**Approved** | [#runtime/36708](https://github.com/dotnet/runtime/issues/36708#issuecomment-636106955) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=0h49m19s)

Looks good as proposed


```diff
 [AttributeUsage(
        AttributeTargets.Field |
        AttributeTargets.ReturnValue |
        AttributeTargets.GenericParameter |
        AttributeTargets.Parameter |
        AttributeTargets.Property |
+       AttributeTargets.Method,
        Inherited = false)]
    public sealed class DynamicallyAccessedMembersAttribute : Attribute
    {
    }
```
## PrincipalPermissionAttribute ctor should be obsolete as error

**Approved** | [#runtime/36972](https://github.com/dotnet/runtime/issues/36972#issuecomment-636122871) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=0h53m31s)

* Looks good as proposed, but we should be adding an URL
* We should also provide a fixer(er) with the potential suggestions (apply ASP.NET Core attribute, using the thread's principal).
* We should consider a generic analyzer that warns on `SecurityAction.Deny` usage across all attributes

```C#
namespace System.Security.Permissions
{
    public partial class PrincipalPermissionAttribute
    {
        [Obsolete("<message goes here>", error: true, DiagnosticId="<diagnostic ID>", UrlFormat="<URL goes here>")]
        public PrincipalPermissionAttribute(SecurityAction action);
    }
}
```
## Use TimeSpan everywhere we use an int for seconds, milliseconds, and timeouts

**Approved** | [#runtime/14336](https://github.com/dotnet/runtime/issues/14336#issuecomment-636135854) | [Video](https://www.youtube.com/watch?v=jhibZCgzB9o&t=1h25m30s)

* We're happy to approve the first group, i.e. a set of overloads to existing methods where `int` is replaced with `TimeSpan`.
* We don't believe properties or using DIMs for interfaces is worth it.

```diff
namespace System {
    public static class GC {
+       public static GCNotificationStatus WaitForFullGCApproach(TimeSpan timeout);
+       public static GCNotificationStatus WaitForFullGCComplete(TimeSpan timeout);
    }
}

namespace System.ComponentModel.DataAnnotations {
    public class RegularExpressionAttribute {
!       This one might not end up being that useful, since people generally don't ever manipulate an instance of this attribute.
+       public TimeSpan MatchTimeout { get; }
    }
}

namespace System.Diagnostics {
    public class Process {
+       public bool WaitForExit(TimeSpan timeout);
+       public bool WaitForInputIdle(TimeSpan timeout);
    }
}

namespace System.IO {
    public class FileSystemWatcher {
+       public WaitForChangedResult WaitForChanged(WatcherChangeTypes changeType, TimeSpan timeout);
    }

    public sealed class NamedPipeClientStream : PipeStream {
+       public void Connect(TimeSpan timeout);
+       public Task ConnectAsync(TimeSpan timeout, CancellationToken cancellationToken = default);
    }
}

namespace System.Net.NetworkInformation {
    public class Ping {
+       public PingReply Send(IPAddress address, TimeSpan timeout, byte[]? buffer = null, PingOptions? options = null);
+       public PingReply Send(string hostNameOrAddress, TimeSpan timeout, byte[]? buffer = null, PingOptions? options = null);

!       Skipped EAP based methods; if they are desired, they can be added back in

!       I added CancellationToken because it is probably worth it. If you don't want it you can remove it.
+       public Task<PingReply> SendPingAsync(IPAddress address, TimeSpan timeout, byte[]? buffer = null, PingOptions? options = null, CancellationToken cancellationToken = default);
+       public Task<PingReply> SendPingAsync(string hostNameOrAddress, TimeSpan timeout, byte[]? buffer = null, PingOptions? options = null, CancellationToken cancellationToken = default);
    }
}

namespace System.Net.Sockets {
    public class NetworkStream : Stream {
+       public void Close(TimeSpan timeout);
    }

    public class Socket {
+       public bool Poll(TimeSpan timeout, SelectMode mode);
+       public static void Select(IList checkRead, IList checkWrite, IList checkError, TimeSpan timeout);
    }
}

namespace System.ServiceProcess {
    public class ServiceBase {
+       public void RequestAdditionalTime(TimeSpan time);
    }
}

namespace System.Threading.Tasks {
    public class Task {
+       public bool Wait(TimeSpan timeout, CancellationToken cancellationToken); 
    } 
} 

namespace System.Timers {
    public class Timer {
+       public Timer(TimeSpan interval);
    }
}
```
