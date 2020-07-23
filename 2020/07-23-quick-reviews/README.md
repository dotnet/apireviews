# Quick Reviews 07/23/2020

## HTTP2: Create additional connections when maximum active streams is reached

**Approved** | [#runtime/35088](https://github.com/dotnet/runtime/issues/35088#issuecomment-663129527) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=0h0m0s)

* Makes sense. We discussed pushing it down but decided against because people are not likely to code against these properties in a generic fashion.

```C#
namespace System.Net.Http
{
    public partial class SocketsHttpHandler : HttpMessageHandler
    {
        public bool EnableMultipleHttp2Connections { get; set; }
    }   
    public partial class WinHttpHandler : HttpMessageHandler
    {
        public bool EnableMultipleHttp2Connections { get; set; }
    }
}
```
## configurable HTTP/2 PING timeouts in HttpClient

**Approved** | [#runtime/31198](https://github.com/dotnet/runtime/issues/31198#issuecomment-663134549) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=0h11m6s)

* Makes sense, but we propose to change `Interval` to `Delay`.

```C#
namespace System.Net.Http
{
    public class SocketsHttpHandler
    {
        public TimeSpan KeepAlivePingDelay { get; set; }
        public TimeSpan KeepAlivePingTimeout { get; set; }
    }
}
```
## BinaryFormatter long-term obsoletion plan

**Approved** | [#designs/141](https://github.com/dotnet/designs/pull/141) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=0h21m16s)

## SocketsHttpHandler feature flag

**Approved** | [#runtime/39489](https://github.com/dotnet/runtime/issues/39489#issuecomment-663144249) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=0h22m14s)

We should consider a factory method like:

```C#
namespace System.Net.Http
{
    public partial class HttpMessageHandler
    {
        public static HttpMessageHandler CreateDefault();
    }
}
```

This would allow library code to write code like this:

```C#
private HttpMessageHandler CreateHandler()
{
    var handler = HttpMessageHandler.CreateDefault();
    if (handler is SocketsHttpHandler socketsHttpHandler)
    {
        // This property will throw PlatformNotSupportedException
        socketsHttpHandler.MaxHttp2ConnectionsPerServer = int.MaxValue;
    }

    return handler;
}
```

However, the proposed API also makes sense:

```C#
namespace System.Net.Http
{
    public partial class SocketsHttpHandler
    {
        public static bool IsSupported { get; }
    }
}
```

@scalablecory, please discuss this with your networking folks and file a separate feature, likely for 6.0.

## API to provide the current system time

**NeedsWork** | [#runtime/36617](https://github.com/dotnet/runtime/issues/36617#issuecomment-663156337) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=0h41m4s)

@maryamariyan @davidfowl would we be OK with a breaking change to reconcile the `ISystemClock` types into a single core type? If not, we shouldn't do this. If yes, then I think we can make progress.
## Unsafe.NullRef, IsNullRef

**Approved** | [#runtime/31170](https://github.com/dotnet/runtime/issues/31170#issuecomment-663159454) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=1h6m33s)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices
{
    public class Unsafe
    {
        public bool IsNullRef<T>(ref T);
        public ref T NullRef<T>();
    }
}
```

## Support for Intel SHA extensions

**Approved** | [#runtime/256](https://github.com/dotnet/runtime/issues/256#issuecomment-663163817) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=1h13m16s)

* Looks good as proposed:
     - Add `IsSupported`
     - Change the `Round` methods to indicate number of rounds
     - Change parameters to match C++ naming
* We should have an analyzer that flags usages, [just like for the AES types](https://github.com/dotnet/roslyn-analyzers/issues/3646). @tannergooding, please file the request and label it with `code-analyzer`.

```C#
namespace System.Runtime.Intrinsics.X86
{
    public class Sha1
    {
        public static bool IsSupported { get; }
        public static Vector128<byte> MessageSchedule1(Vector128<byte> a, Vector128<byte> b);
        public static Vector128<byte> MessageSchedule2(Vector128<byte> a, Vector128<byte> b);
        public static Vector128<byte> NextE(Vector128<byte> a, Vector128<byte> b);
        public static Vector128<byte> FourRounds(Vector128<byte> a, Vector128<byte> b, byte func);
    }

    public class Sha256
    {
        public static bool IsSupported { get; }
        public static Vector128<byte> MessageSchedule1(Vector128<byte> a, Vector128<byte> b);
        public static Vector128<byte> MessageSchedule2(Vector128<byte> a, Vector128<byte> b);
        public static Vector128<byte> TwoRounds(Vector128<byte> a, Vector128<byte> b, Vector128<byte> k);
    }
}
```
## New ArgumentList constructors for Process classes

**Approved** | [#runtime/371](https://github.com/dotnet/runtime/issues/371#issuecomment-663169715) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=1h22m26s)

* The added constructor seems to add little value, the bigger gains would be `Process.Start()`

```C#
namespace System.Diagnostics
{
    public partial class Process
    {
        // Existing overloads:
        // public static Process Start(string fileName);
        // public static Process Start(string fileName, string arguments);
        // public static Process Start(string fileName, string userName, SecureString password, string domain);
        // public static Process Start(string fileName, string arguments, string userName, SecureString password, string domain);
        // public static Process Start(ProcessStartInfo startInfo);
        public static Process Start(string fileName, IEnumerable<string> arguments);
    }
}
```

## Provide x86 CPUID related information

**Approved** | [#runtime/785](https://github.com/dotnet/runtime/issues/785#issuecomment-663178075) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=1h38m51s)

* Looks good as proposed
* All x86 types will extend X86Base (unless it already base type)

```C#
namespace System.Runtime.Intrinsics.X86
{
    public class X86Base
    {
        public static bool IsSupported { get; }

        public static (int Eax, int Ebx, int Ecx, int Edx) CpuId(int functionId, int subFunctionId = 0);
    }
}
```

## Obsolete the SecureString type

**Rejected** | [#runtime/30612](https://github.com/dotnet/runtime/issues/30612#issuecomment-663157607) | [Video](https://www.youtube.com/watch?v=WW65sGkDINQ&t=1h53m9s)

Updated proposal at https://github.com/dotnet/designs/pull/147.
