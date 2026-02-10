# API Review 02/10/2026

## Process: SafeChildProcessHandle and option bag

**Approved** | [#runtime/123380](https://github.com/dotnet/runtime/issues/123380#issuecomment-3880499825) | [Video](https://www.youtube.com/watch?v=C9HbYAApHXg&t=0h0m0s)

* Since this is an options bag, the IList properties can be settable.  Environment has a special object, so it's staying get-only.
* KillOnParentDeath => KillOnParentExit
* Moved the file name resolution into the constructor and eliminated the ResolvePath method.
* We think the API proposed in `SafeChildProcessHandle` should be moved to the existing `SafeProcessHandle` type.
* Expanded Kill with an optional bool to two method groups, `Kill()` and `KillProcessGroup()`
  * And similar for SendSignal, which got renamed to Signal
* Changed ProcessExitStatus to a class, made the ctor public, removed IEquatable.
* WorkingDirectory should be a string, not a DirectoryInfo
* For now, only adding SIGKILL to PosixSignal, can add more later.

```csharp
namespace System.Diagnostics
{
    public sealed class ProcessStartOptions
    {
        public string FileName { get; }
        public IList<string> Arguments { get; set; }
        public IDictionary<string, string?> Environment { get; }
        public string? WorkingDirectory { get; set; }

        public IList<SafeHandle> InheritedHandles { get; set; }
        public bool KillOnParentExit { get; set; }
        public bool CreateNewProcessGroup { get; set; }

        public ProcessStartOptions(string fileName);
    }

    public sealed class ProcessExitStatus
    {
        public ProcessExitStatus(int exitCode, bool cancelled, PosixSignal? signal = null);

        public int ExitCode { get; }
        public PosixSignal? Signal { get; }
        public bool Canceled { get; }
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public partial class SafeProcessHandle
    {
        public int ProcessId { get; }

        public static SafeChildProcessHandle Open(int processId);

        public static SafeChildProcessHandle Start(ProcessStartOptions options, SafeFileHandle? input, SafeFileHandle? output, SafeFileHandle? error);
        public static SafeChildProcessHandle StartSuspended(ProcessStartOptions options, SafeFileHandle? input, SafeFileHandle? output, SafeFileHandle? error);

        public bool Kill();
        public bool KillProcessGroup();
        public void Resume();

        public void Signal(PosixSignal signal);
        public void SignalProcessGroup(PosixSignal signal);

        public ProcessExitStatus WaitForExit();
        public bool TryWaitForExit(TimeSpan timeout, [NotNullWhen(true)] out ProcessExitStatus? exitStatus);
        public ProcessExitStatus WaitForExitOrKillOnTimeout(TimeSpan timeout);

        public Task<ProcessExitStatus> WaitForExitAsync(CancellationToken cancellationToken = default);
        public Task<ProcessExitStatus> WaitForExitOrKillOnCancellationAsync(CancellationToken cancellationToken);
    }
}

namespace System.Runtime.InteropServices
{
    public partial enum PosixSignal
    {
        SIGKILL = -11,
    }
}
```
