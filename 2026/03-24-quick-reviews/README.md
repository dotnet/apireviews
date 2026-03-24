# API Review 03/24/2026

## New Process APIs

**Approved** | [#runtime/125838](https://github.com/dotnet/runtime/issues/125838#issuecomment-4120569132) | [Video](https://www.youtube.com/watch?v=FLO5CXhDYdY&t=0h0m0s)


* Rather than a manual boolean and a collection property for handle inheritance, we went with just a nullable collection.
  * But omitted the prefix "Additional"
* We removed ProcessStartInfo.LeaveHandlesOpen
* We believe KillOnParentExit should be annotated to indicate it only works on Windows and Linux
* Let's leave PostExitDrainTimeout out for now, it needs more data.
* Let's leave SafeFileHandle.IsInheritable out for now, it needs more data.
* There was debate for the ReadAllText and ReadAllBytes returning value-tuples, but we left it doing so (but with correctly cased names)
* Renamed CaptureTextOutput to RunAndCaptureText
* Added the fileName/arguments overload to StartAndForget

Previously approved (but never shipped) API being removed:

```diff
namespace System.Diagnostics
{
-    public sealed partial class ProcessStartOptions
-    {
-        public string FileName { get; }
-        public IList<string> Arguments { get; set; }
-        public IDictionary<string, string?> Environment { get; }
-        public string? WorkingDirectory { get; set; }
-
-        public IList<SafeHandle> InheritedHandles { get; set; }
-        public bool KillOnParentExit { get; set; }
-        public bool CreateNewProcessGroup { get; set; }
-
-        public ProcessStartOptions(string fileName);
-    }

    public sealed partial class ProcessStartInfo
    {
-       public bool InheritHandles { get; set; }
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public partial class SafeProcessHandle
    {
-       public static SafeProcessHandle Start(ProcessStartOptions options, SafeFileHandle? input, SafeFileHandle? output, SafeFileHandle? error);
-       public static SafeProcessHandle StartSuspended(ProcessStartOptions options, SafeFileHandle? input, SafeFileHandle? output, SafeFileHandle? error);

-       public bool Kill();
-       public bool KillProcessGroup();
-       public void Resume();

-       public void Signal(PosixSignal signal);
-       public void SignalProcessGroup(PosixSignal signal);
    }
}
```

```csharp
namespace System.Diagnostics
{
    public sealed partial class ProcessStartInfo
    {
        public SafeFileHandle? StandardInputHandle { get; set; }
        public SafeFileHandle? StandardOutputHandle { get; set; }
        public SafeFileHandle? StandardErrorHandle { get; set; }

        public IList<SafeHandle>? InheritedHandles { get; set; }

        [SupportedOS("windows")]
        [SupportedOS("linux")]
        public bool KillOnParentExit { get; set; }

        public bool StartDetached { get; set; }
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public partial class SafeProcessHandle
    {
        public static SafeProcessHandle Start(ProcessStartInfo startInfo);

        public void Kill();

        public bool Signal(PosixSignal signal);
    }
}

namespace System.Diagnostics
{
    public readonly struct ProcessOutputLine
    {
        public string Content { get; }
        public bool StandardError { get; }
 
        public ProcessOutputLine(string content, bool standardError);
    }

    public sealed class ProcessTextOutput
    {
        public ProcessExitStatus ExitStatus { get; }
        public string StandardOutput { get; }
        public string StandardError { get; }
        public int ProcessId { get; }

        public ProcessTextOutput(ProcessExitStatus exitStatus, string standardOutput, string standardError, int processId);
    }

    public class Process : System.ComponentModel.Component, System.IDisposable
    {
        public IEnumerable<ProcessOutputLine> ReadAllLines(TimeSpan? timeout = default);
        public IAsyncEnumerable<ProcessOutputLine> ReadAllLinesAsync(CancellationToken cancellationToken = default);

        public (string StandardOutput, string StandardError) ReadAllText(TimeSpan? timeout = default);
        public Task<(string StandardOutput, string StandardError)> ReadAllTextAsync(CancellationToken cancellationToken = default);
        public (byte[] StandardOutput, byte[] StandardError) ReadAllBytes(TimeSpan? timeout = default);
        public Task<(byte[] StandardOutput, byte[] StandardError)> ReadAllBytesAsync(CancellationToken cancellationToken = default);

        public static ProcessTextOutput RunAndCaptureText(ProcessStartInfo startInfo, TimeSpan? timeout = default);
        public static ProcessTextOutput RunAndCaptureText(string fileName, IList<string>? arguments = null, TimeSpan? timeout = default);
        public static Task<ProcessTextOutput> RunAndCaptureTextAsync(ProcessStartInfo startInfo, CancellationToken cancellationToken = default);
        public static Task<ProcessTextOutput> RunAndCaptureTextAsync(string fileName, IList<string>? arguments = null, CancellationToken cancellationToken = default);
        public static ProcessExitStatus Run(ProcessStartInfo startInfo, TimeSpan? timeout = default);
        public static ProcessExitStatus Run(string fileName, IList<string>? arguments = null, TimeSpan? timeout = default);
        public static Task<ProcessExitStatus> RunAsync(ProcessStartInfo startInfo, CancellationToken cancellationToken = default);
        public static Task<ProcessExitStatus> RunAsync(string fileName, IList<string>? arguments = null, CancellationToken cancellationToken = default);

        public static int StartAndForget(ProcessStartInfo startInfo);
        public static int StartAndForget(string fileName, IList<string>? arguments = null);
    }
}
```

## Allow ProcessStartInfo and Process.Start arguments to be null

**Approved** | [#runtime/124093](https://github.com/dotnet/runtime/issues/124093#issuecomment-4120585263) | [Video](https://www.youtube.com/watch?v=FLO5CXhDYdY&t=1h32m9s)

Nullablity-only: Looks good as proposed

```diff
namespace System.Diagnostics;

public partial class Process
{
- public ProcessStartInfo(string fileName, string arguments) 
+ public ProcessStartInfo(string fileName, string? arguments) 
- public static Process Start(string fileName, string arguments) 
+ public static Process Start(string fileName, string? arguments) 
}
```
## Tar: archive creation should allow dereferencing soft/hard links to same file

**Approved** | [#runtime/74404](https://github.com/dotnet/runtime/issues/74404#issuecomment-4120743237) | [Video](https://www.youtube.com/watch?v=FLO5CXhDYdY&t=1h35m2s)


* We accepted the feedback that "Ignore" doesn't make sense for hard links, but does for symbolic links, so we split the enums
* Renamed "Strategy" to "Mode" for both the enums and properties
* Renamed "Ignore" to "Skip"

```csharp
namespace System.Formats.Tar
{
    public enum TarSymbolicLinkMode
    {
        PreserveLink,
        CopyContents,
        Skip,
    }

    public enum TarHardLinkMode
    {
        PreserveLink,
        CopyContents,
    }

    // New class
    public sealed class TarWriterOptions
    {
        // Corresponds to existing constructor argument.
        public TarEntryFormat Format { get; set; } = System.Formats.Tar.TarEntryFormat.Pax;

        public TarHardLinkMode HardLinkMode { get; set; } = TarHardLinkMode.PreserveLink;
        public TarSymbolicLinkMode SymbolicLinkMode { get; set; } = TarSymbolicLinkMode.PreserveLink;
    }

    // New class
    public sealed class TarExtractOptions
    {
        // Corresponds to existing ExtractToDirectoryAsync argument.
        public bool OverwriteFiles { get; set; } = false;

        public TarHardLinkMode HardLinkMode { get; set; } = TarHardLinkMode.PreserveLink;
        public TarSymbolicLinkMode SymbolicLinkMode { get; set; } = TarSymbolicLinkMode.PreserveLink;
    }

    public partial class TarWriter
    {
        // New overload accepting options
        public TarWriter(Stream stream, TarWriterOptions options, bool leaveOpen = false);
    }

    public static partial class TarFile
    {
        // New overloads accepting options for creation
        public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarWriterOptions options);
        public static void CreateFromDirectory(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarWriterOptions options);
        public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarWriterOptions options, CancellationToken cancellationToken = default);
        public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarWriterOptions options, CancellationToken cancellationToken = default);

        // New overloads accepting options for extraction
        public static void ExtractToDirectory(Stream source, string destinationDirectoryName, TarExtractOptions options);
        public static void ExtractToDirectory(string sourceFileName, string destinationDirectoryName, TarExtractOptions options);
        public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, TarExtractOptions options, CancellationToken cancellationToken = default);
        public static Task ExtractToDirectoryAsync(string sourceFileName, string destinationDirectoryName, TarExtractOptions options, CancellationToken cancellationToken = default);
    }
}
```

## Add `Utf8JsonWriter.Reset` overloads accepting new `JsonWriterOptions` configuration

**Approved** | [#runtime/123221](https://github.com/dotnet/runtime/issues/123221#issuecomment-4120602344) | [Video](https://www.youtube.com/watch?v=FLO5CXhDYdY&t=2h3m45s)

Looks good as proposed

```csharp
namespace System.Text.Json;

public sealed partial class Utf8JsonWriter : IDisposable, IAsyncDisposable
{
    public void Reset(IBufferWriter bufferWriter, JsonWriterOptions options);
    public void Reset(Stream utf8Json, JsonWriterOptions options);
}
```
