# API Review 03/31/2026

## Add password to ZipArchive

**Approved** | [#runtime/1545](https://github.com/dotnet/runtime/issues/1545#issuecomment-4164739657) | [Video](https://www.youtube.com/watch?v=H5g1DgAxBes&t=0h0m0s)


* Changed EncryptionMethod to ZipEncryptionMethod
* Added ZipArchiveEntry.EncryptionMethod { get; }
* Added Unknown to ZipEncryptionMethod to support it as a signal from the EncryptionMethod property.
* Discussed ReadOnlyMemory vs ReadOnlySpan for ZipArchiveEntry.OpenAsync.  If it's possible to do the KDF in OpenAsync before doing any extra I/O, change the parameter to ReadOnlySpan (even though that's unusual for an Async method)
* Added ZipExtractionOptions and ZipFileCreationOptions to severely reduce the number of overloads.
* Discussed how unhappy we are with creating files using the ZipCrypto algorithm, and decided it doesn't need EB-Never or an analyzer (or to be omitted).  We decided leaving the name as-proposed, not-hidden, and no-analyzer was the correct state for now.

```csharp
namespace System.IO.Compression;

// This enum is intended purely as a user-friendly way to specify the encryption method.
public enum ZipEncryptionMethod
{
    Unknown = -1,
    None = 0,
    ZipCrypto = 1,  // Please doc as for-compat-only.
    Aes128 = 2,
    Aes192 = 3,
    Aes256 = 4,
 }

public partial class ZipArchiveEntry
{
    public ZipEncryptionMethod EncryptionMethod { get; }

    public Stream Open(ReadOnlySpan<char> password);
    public Stream Open(FileAccess access, ReadOnlySpan<char> password);

    // ReadOnlySpan if possible
    public Task<Stream> OpenAsync(ReadOnlyMemory<char> password,
        CancellationToken cancellationToken = default);
    public Task<Stream> OpenAsync(FileAccess access, ReadOnlyMemory<char> password,
        CancellationToken cancellationToken = default);
}

public partial class ZipArchive
{
    public ZipArchiveEntry CreateEntry(string entryName, ReadOnlySpan<char> password, ZipEncryptionMethod encryptionMethod);
    public ZipArchiveEntry CreateEntry(string entryName, CompressionLevel compressionLevel, ReadOnlySpan<char> password, ZipEncryptionMethod encryptionMethod);
}

public sealed class ZipFileCreationOptions
{
    public ReadOnlyMemory<char> Password { get; set; }
    public ZipEncryptionMethod EncryptionMethod { get; set; }
    public CompressionLevel CompressionLevel { get; set; }
    public Encoding? EntryNameEncoding { get; set; }
    public bool IncludeBaseDirectory { get; set; }
}

public sealed class ZipExtractionOptions
{
    public ReadOnlyMemory<char> Password { get; set; }
    public Encoding? EntryNameEncoding { get; set; }
    public bool OverwriteFiles { get; set; }
}

public static partial class ZipFileExtensions
{
    public static ZipArchiveEntry CreateEntryFromFile(this ZipArchive destination, string sourceFileName, string entryName, ReadOnlySpan<char> password, ZipEncryptionMethod encryptionMethod);
    public static ZipArchiveEntry CreateEntryFromFile(this ZipArchive destination, string sourceFileName, string entryName, CompressionLevel compressionLevel, ReadOnlySpan<char> password, ZipEncryptionMethod encryptionMethod);

    // ReadOnlySpan if possible
    public static Task<ZipArchiveEntry> CreateEntryFromFileAsync(this ZipArchive destination, string sourceFileName, string entryName, ReadOnlyMemory<char> password, ZipEncryptionMethod encryptionMethod);
    public static Task<ZipArchiveEntry> CreateEntryFromFileAsync(this ZipArchive destination, string sourceFileName, string entryName, CompressionLevel compressionLevel, ReadOnlyMemory<char> password, ZipEncryptionMethod encryptionMethod);

    public static void ExtractToDirectory(this ZipArchive source, string destinationDirectoryName, ZipExtractionOptions options);
    public static Task ExtractToDirectoryAsync(this ZipArchive source, string destinationDirectoryName, ZipExtractionOptions options, CancellationToken cancellationToken = default);

    public static void ExtractToFile(this ZipArchiveEntry source, string destinationFileName, ZipExtractionOptions options);
    public static Task ExtractToFileAsync(this ZipArchiveEntry source, string destinationFileName, ZipExtractionOptions options, CancellationToken cancellationToken = default);
}

public static partial class ZipFile
{
   public static void ExtractToDirectory(string sourceArchiveFileName, string destinationDirectoryName, ZipExtractionOptions options);
   public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, ZipExtractionOptions options, CancellationToken cancellationToken = default);
   public static void ExtractToDirectory(Stream source, string destinationDirectoryName, ZipExtractionOptions options);
   public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, ZipExtractionOptions options, CancellationToken cancellationToken = default);

    public static void CreateFromDirectory(string sourceDirectoryName, string destinationArchiveFileName, ZipFileCreationOptions options);
    public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, ZipFileCreationOptions options);
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationArchiveFileName, ZipFileCreationOptions options, CancellationToken cancellationToken = default);
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, ZipFileCreationOptions options, CancellationToken cancellationToken = default);
}
```

