# Quick Reviews 03/30/2021

## Add isAsync overload for Create/Open/etc.

**Approved** | [#runtime/24698](https://github.com/dotnet/runtime/issues/24698#issuecomment-810441090) | [Video](https://www.youtube.com/watch?v=DzwksIp-zN4&t=0h0m0s)

Looks good as proposed

```C#
namespace System.IO
{
    public static partial class File
    {
        public StreamWriter AppendText(string path, FileOptions options) => throw null;
        public static System.IO.FileStream Create(string path, System.IO.FileOptions options) => throw null;
        public static System.IO.StreamWriter CreateText(string path, System.IO.FileOptions options) => throw null;   
        public static System.IO.FileStream Open(string path, System.IO.FileMode mode, System.IO.FileOptions options) => throw null;
        public static System.IO.FileStream Open(string path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileOptions options) => throw null;
        public static System.IO.FileStream Open(string path, System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.IO.FileOptions options) => throw null;
        public static System.IO.FileStream OpenRead(string path, System.IO.FileOptions options) => throw null; { throw null; }
        public static System.IO.StreamReader OpenText(string path, System.IO.FileOptions options) => throw null; { throw null; }
        public static System.IO.FileStream OpenWrite(string path, System.IO.FileOptions options) => throw null; { throw null; }
    }
    public sealed partial class FileInfo : System.IO.FileSystemInfo
    {
        public StreamWriter AppendText(FileOptions options) => throw null;
        public System.IO.FileStream Create(System.IO.FileOptions options) => throw null;
        public System.IO.StreamWriter CreateText(System.IO.FileOptions options) => throw null;
        public System.IO.FileStream Open(System.IO.FileMode mode, System.IO.FileOptions options) => throw null;
        public System.IO.FileStream Open(System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileOptions options) => throw null;
        public System.IO.FileStream Open(System.IO.FileMode mode, System.IO.FileAccess access, System.IO.FileShare share, System.IO.FileOptions options) => throw null;
        public System.IO.FileStream OpenRead(System.IO.FileOptions options) => throw null;
        public System.IO.StreamReader OpenText(System.IO.FileOptions options) => throw null;    
        public System.IO.FileStream OpenWrite(System.IO.FileOptions options) => throw null;
    }
    public partial class StreamReader : System.IO.TextReader
    {
        public StreamReader(string path, System.IO.FileOptions options) => throw null;
        public StreamReader(string path, bool detectEncodingFromByteOrderMarks, System.IO.FileOptions options) => throw null;
        public StreamReader(string path, System.Text.Encoding encoding, System.IO.FileOptions options) => throw null;
        public StreamReader(string path, System.Text.Encoding encoding, bool detectEncodingFromByteOrderMarks, System.IO.FileOptions options) => throw null;
        public StreamReader(string path, System.Text.Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize, System.IO.FileOptions options) => throw null;
    }
    public partial class StreamWriter : System.IO.TextWriter
    {
        public StreamWriter(string path, System.IO.FileOptions options) => throw null;
        public StreamWriter(string path, bool append, System.IO.FileOptions options) => throw null;
        public StreamWriter(string path, bool append, System.Text.Encoding encoding, System.IO.FileOptions options) => throw null;
        public StreamWriter(string path, bool append, System.Text.Encoding encoding, int bufferSize, System.IO.FileOptions options) => throw null;
    }
}
```
## FileStream file preallocation performance

**Approved** | [#runtime/45946](https://github.com/dotnet/runtime/issues/45946#issuecomment-810455289) | [Video](https://www.youtube.com/watch?v=DzwksIp-zN4&t=0h22m29s)

* Some of the existing methods are getting really long signatures, let's start defaulting these.
* We asked if we really needed/wanted all 4, and it seemed like a reasonable thing to do.

```C#
namespace System.IO
{
    public class FileStream : Stream
    {
        public FileStream(string path, FileMode mode, FileAccess access = appropriate default, FileShare share = appropriate default, int bufferSize = appropriate default, FileOptions options = appropriate default, long allocationSize = appropriate default)
    }
    
    public partial class StreamWriter : System.IO.TextWriter
    {
        public StreamWriter(string path, bool append = false, System.Text.Encoding? encoding, int bufferSize = -1, FileOptions options = FileOptions.None, long allocationSize = -1)
    }
    
    public static partial class File
    {
        public static FileStream Create(string path, int bufferSize = appropriate default, FileOptions options = appropriate default, long allocationSize = appropriate default)
    }
    
    public sealed partial class FileInfo : System.IO.FileSystemInfo
    {
        public System.IO.FileStream Create(FileOptions options = appropriate default, long allocationSize = appropriate default)
    }
 }
```
## Async File IO APIs mimicking Win32 OVERLAPPED

**NeedsWork** | [#runtime/24847](https://github.com/dotnet/runtime/issues/24847#issuecomment-810484866) | [Video](https://www.youtube.com/watch?v=DzwksIp-zN4&t=0h44m41s)

* In the review we were generally (but not unanimously) against the new types
* The `SafeFileHandle` extensions approach was received more warmly
* Instead of extension methods, consider just static invocations to allow for variations.
* Consider an `IsSupported`-type API so that it's clear they don't work on mobile.

Something like

```C#
namespace System.IO // TBD
{
    public static class FileHandleOperations
    {
        public static bool SupportsOffsetOperations { get; }

        public static int ReadAtOffset(SafeFileHandle fileHandle, Span<byte> buffer, ulong fileOffset);
        public static int ReadAtOffset(IntPtr fileHandle, Span<byte> buffer, ulong fileOffset);

        public static int WriteAtOffset(SafeFileHandle fileHandle, ReadOnlySpan<byte> buffer, ulong fileOffset);
        public static int WriteAtOffset(IntPtr fileHandle, ReadOnlySpan<byte> buffer, ulong fileOffset);
         
        public static ValueTask<int> ReadAtOffsetAsync(SafeFileHandle fileHandle, Memory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<int> ReadAtOffsetAsync(IntPtr fileHandle, Memory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);

        public static ValueTask<int> WriteAtOffsetAsync(SafeFileHandle fileHandle, ReadOnlyMemory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<int> WriteAtOffsetAsync(IntPtr fileHandle, ReadOnlyMemory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);
    }
}
```

but that needs to be verified against the original scenarios.
## Add class as a target for RequiresUnreferencedCodeAttribute

**Approved** | [#runtime/50122](https://github.com/dotnet/runtime/issues/50122#issuecomment-810493381) | [Video](https://www.youtube.com/watch?v=DzwksIp-zN4&t=1h29m20s)

* Looks good as proposed
* We discussed the Struct and Interface targets, and felt that while they'll eventually show up we shouldn't add the usage until we have an implementation backing them up.

```diff
 namespace System.Diagnostics.CodeAnalysis
 {
     [AttributeUsage(
+        AttributeTargets.Class |
         AttributeTargets.Constructor |
         AttributeTargets.Method,
         Inherited = false)]
     public sealed class RequiresUnreferencedCodeAttribute : Attribute
     {
     }
 }
````
## Add compression support in WebSocket

**Approved** | [#runtime/31088](https://github.com/dotnet/runtime/issues/31088#issuecomment-810509834) | [Video](https://www.youtube.com/watch?v=DzwksIp-zN4&t=1h42m48s)

* We discussed whether the DisableCompression should be Enable or Disable, and agree Disable makes sense as a per-message option
* The WebSocketMessageFlags enum should name the zero member (None, Default, etc)
* Consider defaulting the WebSocketMessageFlags parameter in SendAsync
* We're OK with the new "Dangerous" prefix, so long as it's really expected to be rarely used -- don't dilute the meaning of "Dangerous".
  * Ensure that there's consistency or followup action for higher level APIs that may be doing similar things, such as SignalR/ASP.NET.


```diff
namespace System.Net.WebSockets
{
    public sealed class WebSocketCreationOptions
    {
        public bool IsServer { get; set; }
        public string? SubProtocol { get; set; }
        public TimeSpan KeepAliveInterval { get; set; }
-       public WebSocketDeflateOptions? DeflateOptions { get; set; } = null;
+       public WebSocketDeflateOptions? DangerousDeflateOptions { get; set; } = null;
    }

    public partial class ClientWebSocketOptions
    {
-       public WebSocketDeflateOptions? DeflateOptions { get; set; } = null;
+       public WebSocketDeflateOptions? DangerousDeflateOptions { get; set; } = null;
    }

+   [Flags]
+   public enum WebSocketMessageFlags
+   {
+       None = 0,
+
+       // taken from existing WebSocket.SendAsync params
+       EndOfMessage = 1,
+
+       // new
+       DisableCompression = 2
+   }

    public partial class WebSocket
    {
+       public virtual ValueTask SendAsync(ReadOnlyMemory<byte> buffer, WebSocketMessageType messageType, WebSocketMessageFlags messageFlags, CancellationToken cancellationToken = default);
    }    
}
```
