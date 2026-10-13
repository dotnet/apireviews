# API Review 10/13/2026

## Change ZstandardDictionary.Train to return byte[]

**Approved** | [#runtime/135265](https://github.com/dotnet/runtime/issues/135265#issuecomment-6151031990) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=0h0m0s)

* Looks good as proposed

```diff
using System.IO.Compression;

public partial class ZstandardDictionary
{
-   public static ZstandardDictionary Train(ReadOnlySpan<byte> samples, ReadOnlySpan<int> sampleLengths, int maxDictionarySize);
+   public static byte[] Train(ReadOnlySpan<byte> samples, ReadOnlySpan<int> sampleLengths, int maxDictionarySize);
}
```
## Add support for Brotli Large Window decompression

**Approved** | [#runtime/133018](https://github.com/dotnet/runtime/issues/133018#issuecomment-6151205781) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=0h7m33s)

Looks good as proposed.

```csharp
namespace System.IO.Compression
{
    public sealed class BrotliDecompressionOptions
    {
        public BrotliDecompressionOptions();
        public int MaxWindowLog2 { get; set; } // default: 24
    }

    public partial struct BrotliDecoder : IDisposable
    {
        public BrotliDecoder(BrotliDecompressionOptions decompressionOptions);
    }

    public sealed partial class BrotliStream : Stream
    {
        public BrotliStream(Stream stream, BrotliDecompressionOptions decompressionOptions, bool leaveOpen = false);
    }
}
```
## Obsolete HybridCache compression options

**Approved** | [#extensions/6594](https://github.com/dotnet/extensions/issues/6594#issuecomment-6151294974) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=0h17m15s)

Looks good as proposed

```diff
 namespace Microsoft.Extensions.Caching.Hybrid;

 public class HybridCacheOptions
 {
+    [Obsolete(
+        "HybridCache does not provide built-in compression, so this option doesn't do anything.",
+        DiagnosticId = "EXTOBS0003",
+        UrlFormat = "https://aka.ms/dotnet-extensions-warnings/{0}")]
+    [EditorBrowsable(EditorBrowsableState.Never)]
     public bool DisableCompression { get; set; }
 }
```
## Obsolete RegistryKeyPermissionCheck and defer registry write checks to the OS

**Approved** | [#runtime/113535](https://github.com/dotnet/runtime/issues/113535#issuecomment-6152017402) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=0h22m18s)

* As far as we can tell, the `writable` value applies to an intent of the .NET object holder, which is distinct from the Windows registry permissions.
  * If that's the case, then we shouldn't both add new overloads with `bool writable` AND remove EnsureWritable checks.
  * There should be a pass over the documentation to make it clearer what the `writable` does, making clear distinction between the registry permissions, a local object intent/restriction, and a SafeHandle (if that's even exposed)'s access rights.
* Let's not Obsolete the enum, but rather all methods that use it, and ensure they all have `bool writable` overloads.

```diff
namespace Microsoft.Win32
{
    public sealed partial class RegistryKey
    {
+       [Obsolete(...)]
        public RegistryKey CreateSubKey(
            string subkey,
            RegistryKeyPermissionCheck permissionCheck);

+       [Obsolete(...)]
        public RegistryKey CreateSubKey(
            string subkey,
            RegistryKeyPermissionCheck permissionCheck,
            RegistrySecurity? registrySecurity);

+       [Obsolete(...)]
        public RegistryKey CreateSubKey(
            string subkey,
            RegistryKeyPermissionCheck permissionCheck,
            RegistryOptions options);

+       [Obsolete(...)]
        public RegistryKey CreateSubKey(
            string subkey,
            RegistryKeyPermissionCheck permissionCheck,
            RegistryOptions options,
            RegistrySecurity? registrySecurity);

+       [Obsolete(...)]
        public RegistryKey? OpenSubKey(
            string name,
            RegistryKeyPermissionCheck permissionCheck);

+       [Obsolete(...)]
        public RegistryKey? OpenSubKey(
            string name,
            RegistryKeyPermissionCheck permissionCheck,
            RegistryRights rights);
    }
}
```

```csharp
namespace Microsoft.Win32
{
    public sealed partial class RegistryKey
    {
        public RegistryKey CreateSubKey(
            string subkey,
            bool writable,
            RegistrySecurity? registrySecurity);

        public RegistryKey CreateSubKey(
            string subkey,
            bool writable,
            RegistryOptions options,
            RegistrySecurity? registrySecurity);

        public RegistryKey? OpenSubKey(
            string name,
            bool writable,
            RegistryRights rights);
    }
}
```
## Base64Url option to emit padding

**Approved** | [#runtime/129847](https://github.com/dotnet/runtime/issues/129847#issuecomment-6152367131) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=1h2m54s)


* We changed Base64UrlOptions to Base64UrlFormattingOptions for better alignment with Base64FormattingOptions
* InsertPadding => IncludePadding (Padding is Appended, not Inserted)
* In case we ever want to support inserting linebreaks, let's leave 1 free so the option doesn't need translation across Base64FormattingOptions and Base64UrlFormattingOptions

```csharp
namespace System.Buffers.Text;

[Flags]
public enum Base64UrlFormattingOptions
{
    None = 0,
    // 1 is reserved for the future, if we add Linebreak.
    IncludePadding = 2,
}

public static partial class Base64Url
{
    public static string EncodeToString(ReadOnlySpan<byte> source, Base64UrlFormattingOptions options);
    public static byte[] EncodeToUtf8(ReadOnlySpan<byte> source, Base64UrlFormattingOptions options);
    public static char[] EncodeToChars(ReadOnlySpan<byte> source, Base64UrlFormattingOptions options);

    public static int EncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination, Base64UrlFormattingOptions options);
    public static int EncodeToChars(ReadOnlySpan<byte> source, Span<char> destination, Base64UrlFormattingOptions options);

    public static bool TryEncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, Base64UrlFormattingOptions options);
    public static bool TryEncodeToChars(ReadOnlySpan<byte> source, Span<char> destination, out int charsWritten, Base64UrlFormattingOptions options);
    public static bool TryEncodeToUtf8InPlace(Span<byte> buffer, int dataLength, out int bytesWritten, Base64UrlFormattingOptions options);

    public static OperationStatus EncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, Base64UrlFormattingOptions options, bool isFinalBlock = true);
    public static OperationStatus EncodeToChars(ReadOnlySpan<byte> source, Span<char> destination, out int bytesConsumed, out int charsWritten, Base64UrlFormattingOptions options, bool isFinalBlock = true);

    public static int GetEncodedLength(int bytesLength, Base64UrlFormattingOptions options);
}
```
## Add Math.DivRem and Math.IEEEDivRem for floating point types

**Rejected** | [#runtime/122855](https://github.com/dotnet/runtime/issues/122855#issuecomment-6152516262) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=1h22m23s)

The `Math` class isn't gaining new members any more.  If we were going to add these methods, they would be static methods on float and double individually.

That said, the space that needs these is somewhat niche, and the presentation of it likely adds more confusion (and maintenance burden) than it adds in value.

There doesn't seem to be enough evidence to warrant the inclusion of this in the platform.
## Plane.Project

**Approved** | [#runtime/107407](https://github.com/dotnet/runtime/issues/107407#issuecomment-6152762317) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=1h30m33s)

Looks good as proposed.

```csharp
namespace System.Numerics;

public partial struct Plane
{
    public static Vector3 Project(Plane plane, Vector3 point);
}
```
## Add a new `CanonicalizeHostName` property on `LdapSessionOptions`

**Approved** | [#runtime/125454](https://github.com/dotnet/runtime/issues/125454#issuecomment-6153061602) | [Video](https://www.youtube.com/watch?v=R-o4X4fSxqM&t=1h44m25s)

* We debated if we wanted a different name based on what the default is, but this type appears to have a mix of default true and default false options already, so a new default true one is fine.
  * In case Windows is actually false, consider changing Linux to also default false.

```csharp
namespace System.DirectoryServices.Protocols;

public class LdapSessionOptions
{
    public bool CanonicalizeHostName
    {
        get; 
        [UnsupportedOSPlatform("windows")]
        set;
    }
}
```
