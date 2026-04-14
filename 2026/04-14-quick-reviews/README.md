# API Review 04/14/2026

## UnionAttribute and IUnion interface

**Approved** | [#runtime/126650](https://github.com/dotnet/runtime/issues/126650#issuecomment-4245829272) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=0h0m0s)

Looks good as proposed

```csharp
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(Class | Struct, AllowMultiple = false)]
    public class UnionAttribute : Attribute;

    public interface IUnion
    {
        // The value of the union or null
        object? Value { get; }
    }
}
```
## Add XZ (LZMA) compression support to System.IO.Compression

**Approved** | [#runtime/1542](https://github.com/dotnet/runtime/issues/1542#issuecomment-4245991693) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=0h13m3s)

Looks good as proposed

```csharp
namespace System.IO.Compression
{
  public enum XzChecksumType
  {
      None = 0,
      Crc32 = 1,
      Crc64 = 2,
      Sha256 = 3,
  }
  public sealed partial class XzCompressionOptions
  {
      public XzCompressionOptions() { }
      public static int DefaultQuality { get { throw null; } }
      public static int DefaultWindowLog { get { throw null; } }
      public static int MaxQuality { get { throw null; } }
      public static int MaxWindowLog { get { throw null; } }
      public static int MinQuality { get { throw null; } }
      public static int MinWindowLog { get { throw null; } }
      public System.IO.Compression.XzChecksumType Checksum { get { throw null; } set { } }
      public int WindowLog { get { throw null; } set { } }
      public bool EnableExtremeMode { get { throw null; } set { } }
      public int Quality { get { throw null; } set { } }
  }
  public sealed partial class XzDecoder : System.IDisposable
  {
      public XzDecoder() { }
      public XzDecoder(int maxWindowLog) { }
      public System.Buffers.OperationStatus Decompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten) { throw null; }
      public void Dispose() { }
      public static bool TryDecompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten) { throw null; }
  }
  public sealed partial class XzEncoder : System.IDisposable
  {
      public XzEncoder() { }
      public XzEncoder(int quality) { }
      public XzEncoder(int quality, int windowLog) { }
      public XzEncoder(System.IO.Compression.XzCompressionOptions compressionOptions) { }
      public System.Buffers.OperationStatus Compress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock) { throw null; }
      public void Dispose() { }
      public System.Buffers.OperationStatus Flush(System.Span<byte> destination, out int bytesWritten) { throw null; }
      public static long GetMaxCompressedLength(long inputLength) { throw null; }
      public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten) { throw null; }
      public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality) { throw null; }
      public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality, int windowLog) { throw null; }
  }
  public sealed partial class XzStream : System.IO.Stream
  {
      public XzStream(System.IO.Stream stream, System.IO.Compression.CompressionLevel compressionLevel, bool leaveOpen = false) { }
      public XzStream(System.IO.Stream stream, System.IO.Compression.CompressionMode mode, bool leaveOpen = false) { }
      public XzStream(System.IO.Stream stream, System.IO.Compression.XzCompressionOptions compressionOptions, bool leaveOpen = false) { }
      public System.IO.Stream BaseStream { get { throw null; } }
  }
}  
```
## Add async overload of ChangeToken.OnChange

**Approved** | [#runtime/69099](https://github.com/dotnet/runtime/issues/69099#issuecomment-4246133356) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=0h41m34s)

* We discussed CancellationToken, and while it's not visible in the API, it's possible (without capture) via the TState overload.
* We discussed whether the async-void parallel vs serial behavior was good to do (overload) or bad (new method group) and feel that an overload is better.

```csharp
 namespace Microsoft.Extensions.Primitives;

 public static class ChangeToken
 {
    public static IDisposable OnChange(
        Func<IChangeToken?> changeTokenProducer, Func<Task> changeTokenConsumer);
    public static IDisposable OnChange<TState>(
        Func<IChangeToken?> changeTokenProducer, Func<TState, Task> changeTokenConsumer, TState state);
 }
```
## Replacement of forbidden characters in variable names in EnvironmentVariablesConfigurationProvider

**Approved** | [#runtime/125137](https://github.com/dotnet/runtime/issues/125137#issuecomment-4246306609) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=1h6m30s)


* We added statics for the default transformation and the most common expected case (colon and dot).
* We believe that assigning VariableNameTransformation always replaces the default behavior, it's not a pretransformation.

```csharp
namespace Microsoft.Extensions.Configuration.EnvironmentVariables
{
    public partial class EnvironmentVariablesConfigurationSource : IConfigurationSource
    {
        public static Func<string, string> DefaultTransformation { get; }
        public static Func<string, string> ColonAndDotTransformation { get; }        

        public Func<string, string>? VariableNameTransformation { get; set; }
    }
}
namespace Microsoft.Extensions.Configuration
{
    public partial static class EnvironmentVariablesExtensions
    {
        public static IConfigurationBuilder AddEnvironmentVariables(
            this IConfigurationBuilder configurationBuilder, string? prefix, Func<string, string>? variableNameTransformation);
    }
}
```
## Add Registered ID attribute to SubjectAlternativeNameBuilder

**Approved** | [#runtime/125127](https://github.com/dotnet/runtime/issues/125127#issuecomment-4246341943) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=1h37m29s)

* We discussed the various alternatives for the parameter name, and landed on registeredId.
* We fixed the casing on "ID" to "Id"

```csharp
namespace System.Security.Cryptography.X509Certificates;

public sealed class SubjectAlternativeNameBuilder
{
    public void AddRegisteredId(string registeredId);
}
```
## Type.GetNullableUnderlyingType()

**Approved** | [#runtime/125388](https://github.com/dotnet/runtime/issues/125388#issuecomment-4246444558) | [Video](https://www.youtube.com/watch?v=L0LeEmrb9I8&t=1h44m59s)

The name has a small amount of semantic ambiguity (is it returning a null value instead of throwing? Or is it the underlying type of `Nullable<T>`?  The latter.), but it's niche, and matches GetEnumUnderlyingType.

```csharp
namespace System;

public abstract class Type
{
    /// <summary>
    /// Returns the underlying type argument of a <see cref="Nullable{T}"/> type.
    /// </summary>
    /// <returns>
    /// The type argument if the current type is a closed generic Nullable{T};
    /// otherwise, null.
    /// </returns>
    public virtual Type? GetNullableUnderlyingType();
}
```
