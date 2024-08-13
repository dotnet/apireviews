# API Review 08/13/2024

## TypeName.MakeSimpleTypeName(AssemblyNameInfo) to avoid type name reparsing

**Approved** | [#runtime/106022](https://github.com/dotnet/runtime/issues/106022#issuecomment-2286735235) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Reflection.Metadata;

public partial class TypeName
{
    public TypeName WithAssemblyName(AssemblyNameInfo? assemblyInfo);
}
```
## Expose TLS details for QUIC connection

**Approved** | [#runtime/70184](https://github.com/dotnet/runtime/issues/70184#issuecomment-2286742126) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h7m37s)

* Looks good as proposed

```C#
namespace System.Net.Quic;

public partial class QuicConnection
{
    public SslProtocols SslProtocol { get; }

    [CLSCompliant(false)]
    public TlsCipherSuite NegotiatedCipherSuite { get; }
}
```
## public ctor for CookieException with string parameter

**Approved** | [#runtime/95965](https://github.com/dotnet/runtime/issues/95965#issuecomment-2286751554) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h11m44s)

* We should also add the one that takes inner exceptions, for consistency with other exceptions

```C#
namespace System.Net;

public partial class CookieException
{
    // Existing ctors:
    // public CookieException();
    // public CookieException(SerializationInfo, StreamingContext);
    public CookieException(string? message);
    public CookieException(string? message, Exception? innerException);
 }
```
## AggregateRateLimiter

**Approved** | [#runtime/98017](https://github.com/dotnet/runtime/issues/98017#issuecomment-2286799314) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h16m58s)

* We'd prefer the method, due to prior art

```C#
namespace System.Threading.RateLimiting;

public abstract class RateLimiter
{
    public static RateLimiter CreateChained(params RateLimiter[] limiters);
}
```
## [Analyzer] Flagging issues in logger message templates w/ incomplete braces pairs

**Approved** | [#runtime/101698](https://github.com/dotnet/runtime/issues/101698#issuecomment-2286809375) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h43m3s)

This analyzer makes sense. Should probably be a different diagnostic ID but can likely be handled inside the existing analyzer for the argument validation.

We should use same category and same severity as for the other one that validates number of arguments.
## Add a DirectoryPath property to DirectoryNotFoundException to be consistent with FileNotFoundException

**Approved** | [#runtime/103468](https://github.com/dotnet/runtime/issues/103468#issuecomment-2286841778) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=0h48m25s)

* We discussed making the property non-null but decided it was simpler to just store what we were given
    - Also consistent with `FileNotFoundException`
* If `message` is `null` but a `directoryPath` is given `Message` should return a constructed one, similar to `FileNotFoundException`

```C#
namespace System.IO;

public partial class DirectoryNotFoundException
{
    // Existing ctors
    // public DirectoryNotFoundException();
    // public DirectoryNotFoundException(String);
    // public DirectoryNotFoundException(SerializationInfo, StreamingContext);
    // public DirectoryNotFoundException(String, Exception);
    public DirectoryNotFoundException(string? message, string? directoryPath);
    public DirectoryNotFoundException(string? message, string? directoryPath, Exception? innerException);
    public string? DirectoryPath { get; }
}
```
## Add FromFile method to BinaryData

**Approved** | [#runtime/106203](https://github.com/dotnet/runtime/issues/106203#issuecomment-2286861389) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=1h7m23s)

* We should also add `mediaType` to the sync overload

```C#
namespace System.Memory.Data;

public partial class BinaryData
{
    public static BinaryData FromFile(string path);
    public static BinaryData FromFile(string path, string? mediaType);
    public static Task<BinaryData> FromFileAsync(string path, CancellationToken cancellationToken = default);
    public static Task<BinaryData> FromFileAsync(string path, string? mediaType, CancellationToken cancellationToken = default);
}
```
## Add Vector Embedding Type

**NeedsWork** | [#runtime/102669](https://github.com/dotnet/runtime/issues/102669) | [Video](https://www.youtube.com/watch?v=PNvzs8aBGD4&t=1h18m17s)

