# API Review 06/23/2026

## Process constructor from SafeProcessHandle

**Approved** | [#runtime/71595](https://github.com/dotnet/runtime/issues/71595#issuecomment-4782140116) | [Video](https://www.youtube.com/watch?v=4EHES3BcgBY&t=0h0m0s)


* Changed WindowsProcessStartArguments.Start.callback to return nint instead of SafeProcessHandle, for consistency and safe SafeHandle construction.
* Moved all of the supported/unsupported annotations to the same location instead of split.
* We discussed the pointer exposure, and left it as-is since the only use case for these types is P/Invoke
* System.Runtime.InteropServices seems like the best namespace of ones that exist, and it doesn't seem worthy of a new one.

```csharp
namespace System.Runtime.InteropServices;

public ref struct WindowsProcessStartArguments
{
    public WindowsProcessStartArguments();

    public unsafe char* Arguments { get; }
    public unsafe char* EnvironmentVariables{ get; }

    public ProcessStartInfo ProcessStartInfo { get; }
    public nint StandardError { get; }
    public nint StandardInput { get; }
    public nint StandardOutput { get; }

    [SupportedOSPlatform("windows")]
    public static Process Start(ProcessStartInfo startInfo, Func<WindowsProcessStartArguments, nint> callback);
}

public ref struct UnixProcessStartArguments
{
    public UnixProcessStartArguments();

    public unsafe byte* ResolvedPath { get; }
    public unsafe byte** Arguments { get; }
    public unsafe byte** EnvironmentVariables { get; }

    public ProcessStartInfo ProcessStartInfo { get; }
    public nint StandardError { get; }
    public nint StandardInput { get; }
    public nint StandardOutput { get; }

    [UnsupportedOSPlatform("windows")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [SupportedOSPlatform("maccatalyst")]
    public static Process Start(ProcessStartInfo startInfo, Func<UnixProcessStartArguments, int> callback);
}
```

## ZstandardDecompressionOptions

**Approved** | [#runtime/128456](https://github.com/dotnet/runtime/issues/128456#issuecomment-4782229064) | [Video](https://www.youtube.com/watch?v=4EHES3BcgBY&t=1h6m38s)

* Sealed the options type
* Since the Zstandard types haven't shipped yet, consider a preview breaking change of removing the (Int32), (ZstandardDictionary), and (ZstandardDictionary, Int32) constructors so there is only default and with this options class.
* Feedback came up that "WindowLog" looks too much like logging, since this is new API consider renaming to "MaxWindowLog2" (here and with Brotli, et al)

```csharp
namespace System.IO.Compression;

public sealed class ZstandardDecompressionOptions
{
    public ZstandardDictionary? Dictionary { get; set; }

    // The maximum allowed window size when decompressing payloads, expressed as base 2 logarithm.
    // Value 0 indicates the implementation-defined default window size.
    // range from ZstandardCompressionOptions.MinWindowLog - MaxWindowLog
    public int MaxWindowLog { get; set; } 
}

public partial class ZstandardDecoder
{
    // not strictly necessary, other ctors can be used, but will complete the API symmetry with encoder options
    public ZstandardDecoder(ZstandardDecompressionOptions decompressionOptions);
}

public partial class ZstandardStream
{
    // symmetric to ZstandardStream(Stream! stream, ZstandardCompressionOptions! compressionOptions, bool leaveOpen = false);
    public ZstandardStream(Stream stream, ZstandardDecompressionOptions decompressionOptions, bool leaveOpen = false);
}
```
## Obsolete AsnEncodedData RawData setter

**Approved** | [#runtime/129544](https://github.com/dotnet/runtime/issues/129544#issuecomment-4782319367) | [Video](https://www.youtube.com/watch?v=4EHES3BcgBY&t=1h18m37s)

* Obsoleting it seems appropriate.
* Use the next available SYSLIB value

```diff
namespace System.Security.Cryptography

public partial class AsnEncodedData {
    public byte[] RawData {
        get;
+       [Obsolete("Setting RawData is obsolete. Use CopyFrom instead.", DiagnosticId = "SYSLIBxxxx", ...)]
        set;
    }
}
```
## Add `ToLowerOrdinal()` & `ToUpperOrdinal()`

**Approved** | [#runtime/90999](https://github.com/dotnet/runtime/issues/90999#issuecomment-4782378274) | [Video](https://www.youtube.com/watch?v=4EHES3BcgBY&t=1h29m57s)

Looks good as proposed.

```csharp
 namespace System;

 public sealed partial class String
 {
    public string ToLowerOrdinal();
    public string ToUpperOrdinal();
 }

 public readonly partial struct Char
 {
    public static char ToLowerOrdinal(char c);
    public static char ToUpperOrdinal(char c);
 }

 namespace System.Text;

 public readonly partial struct Rune
 {
    public static Rune ToLowerOrdinal(Rune value);
    public static Rune ToUpperOrdinal(Rune value);
 }

 namespace System;

 public static partial class MemoryExtensions
 {
    public static int ToLowerOrdinal(this ReadOnlySpan<char> source, Span<char> destination);
    public static int ToUpperOrdinal(this ReadOnlySpan<char> source, Span<char> destination);
 }
```
## AesGcm/AesCcm.KeySize

**Approved** | [#runtime/72191](https://github.com/dotnet/runtime/issues/72191#issuecomment-4782446055) | [Video](https://www.youtube.com/watch?v=4EHES3BcgBY&t=1h37m14s)

* Changed KeySize to KeySizeInBytes

```csharp
namespace System.Security.Cryptography;

public partial class AesGcm
{
    public int KeySizeInBytes { get; }
}

public partial class AesCcm
{
    public int KeySizeInBytes { get; }
}
```
