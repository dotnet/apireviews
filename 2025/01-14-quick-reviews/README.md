# API Review 01/14/2025

## AssemblyLoadContext.TryLoadFromAssemblyName

**Approved** | [#runtime/87185](https://github.com/dotnet/runtime/issues/87185#issuecomment-2590920058) | [Video](https://www.youtube.com/watch?v=UB1FXGlxqkA&t=0h0m0s)

* The new method will behave such that it returns `false` if the underlying API returned `null` or threw `FileNotFoundException`
    - That means it won't eliminate all exceptions but most
    - We can try to fix other resolvers to return `null` instead of throwing

```C#
namespace System.Runtime.Loader;

public partial class AssemblyLoadContext
{
    public bool TryLoadFromAssemblyName(AssemblyName assemblyName, [NotNullWhen(true)] out Assembly? assembly);
}
```
## IPAddress.IsValid

**Approved** | [#runtime/111282](https://github.com/dotnet/runtime/issues/111282#issuecomment-2590938639) | [Video](https://www.youtube.com/watch?v=UB1FXGlxqkA&t=0h44m2s)

* We'd prefer to rename the second one because there is a constructor that takes a byte span which is expected to be the binary encoding

```C#
namespace System.Net;

public partial class IPAddress
{
    // Existing:
    // public static IPAddress Parse(string ipString);
    // public static IPAddress Parse(ReadOnlySpan<char> ipSpan);
    // public static IPAddress Parse(ReadOnlySpan<byte> utf8Text);
    // public static bool TryParse(string? ipString, out IPAddress? address);
    // public static bool TryParse(ReadOnlySpan<char> ipSpan, out IPAddress? address);
    // public static bool TryParse(ReadOnlySpan<byte> utf8Text, out IPAddress? result);

    public static bool IsValid(ReadOnlySpan<char> ipSpan);
    public static bool IsValidUtf8(ReadOnlySpan<byte> utf8Text);
}
```
## Provide an implementation AesGcm in Microsoft.Bcl.Cryptography

**Approved** | [#runtime/89718](https://github.com/dotnet/runtime/issues/89718#issuecomment-2591068054) | [Video](https://www.youtube.com/watch?v=UB1FXGlxqkA&t=0h54m54s)

* We only expose the good ctors
* Practically, this means this package supports .NET Framework and .NET 8.0+, and offers .NET Standard 2.0 only as an easy way to target both from a single library
* We should raise a build error when consumed by an unsupported target framework (such as .NET Standard 2.1)

```C#
namespace System.Security.Cryptography;

public sealed partial class AesGcm : System.IDisposable
{
    public AesGcm(byte[] key, int tagSizeInBytes);
    public AesGcm(ReadOnlySpan<byte> key, int tagSizeInBytes);
    public static bool IsSupported { get; }
    public static KeySizes NonceByteSizes { get; }
    public static KeySizes TagByteSizes { get; }
    public int? TagSizeInBytes { get; }
    public void Decrypt(byte[] nonce, byte[] ciphertext, byte[] tag, byte[] plaintext, byte[]? associatedData = null);
    public void Decrypt(ReadOnlySpan<byte> nonce, ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> tag, Span<byte> plaintext, ReadOnlySpan<byte> associatedData = default(ReadOnlySpan<byte>));
    public void Dispose();
    public void Encrypt(byte[] nonce, byte[] plaintext, byte[] ciphertext, byte[] tag, byte[]? associatedData = null);
    public void Encrypt(ReadOnlySpan<byte> nonce, ReadOnlySpan<byte> plaintext, Span<byte> ciphertext, Span<byte> tag, ReadOnlySpan<byte> associatedData = default(ReadOnlySpan<byte>));
}

public sealed partial class AuthenticationTagMismatchException : CryptographicException
{
    public AuthenticationTagMismatchException();
    public AuthenticationTagMismatchException(string message);
    public AuthenticationTagMismatchException(string message, System.Exception inner);
}
```
