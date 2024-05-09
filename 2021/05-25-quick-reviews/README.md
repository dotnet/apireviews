# Quick Reviews 05/25/2021

## Async File IO APIs mimicking Win32 OVERLAPPED

**Approved** | [#runtime/24847](https://github.com/dotnet/runtime/issues/24847#issuecomment-848063507) | [Video](https://www.youtube.com/watch?v=kO1HyNmi7ww&t=0h0m0s)

* On re-review we decided to remove the names Scatter and Gather, as we already have precedent for these concepts without the name.
* On re-review we feel that, on the whole, the "AtOffset" particles aren't necessary with the static method invocation.  Since there's a fair hesitation for having these as instance methods on the SafeHandle type or even extension methods, we can go ahead and remove the redundancy.

```C#
namespace System.IO
{
    public static class RandomAccess
    {
        public static long GetLength(SafeFileHandle handle) => throw null;

        public static int Read(SafeFileHandle handle, Span<byte> buffer, long fileOffset);
        public static int Read(SafeFileHandle handle, Span<byte> buffer, ulong fileOffset);
        public static void Write(SafeFileHandle handle, ReadOnlySpan<byte> buffer, long fileOffset);
        public static void Write(SafeFileHandle handle, ReadOnlySpan<byte> buffer, ulong fileOffset);

        public static long Read(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, long fileOffset);
        public static long Read(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, ulong fileOffset);
        public static void Write(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, long fileOffset);
        public static void Write(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, ulong fileOffset);
    
        public static ValueTask<int> ReadAsync(SafeFileHandle handle, Memory<byte> buffer, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<int> ReadAsync(SafeFileHandle handle, Memory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAsync(SafeFileHandle handle, ReadOnlyMemory<byte> buffer, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAsync(SafeFileHandle handle, ReadOnlyMemory<byte> buffer, ulong fileOffset, CancellationToken cancellationToken = default);

        public static ValueTask<long> ReadAsync(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask<long> ReadAsync(SafeFileHandle handle, IReadOnlyList<Memory<byte>> buffers, ulong fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAsync(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, long fileOffset, CancellationToken cancellationToken = default);
        public static ValueTask WriteAsync(SafeFileHandle handle, IReadOnlyList<ReadOnlyMemory<byte>> buffers, ulong fileOffset, CancellationToken cancellationToken = default);
    }

    partial class File
    {
        public static SafeFileHandle OpenHandle(string filePath, FileMode mode = FileMode.Open, FileAccess access = FileAccess.Read, FileShare share = FileShare.Read, FileOptions options = FileOptions.None, long preAllocationSize = 0)
    }
}

namespace Microsoft.Win32.SafeHandles
{
    partial class SafeFileHandle
    {
        public bool IsAsync { get; }
    }
}
```
## String.{Try}CopyTo(Span<char>)

**Approved** | [#runtime/51061](https://github.com/dotnet/runtime/issues/51061#issuecomment-848073697) | [Video](https://www.youtube.com/watch?v=kO1HyNmi7ww&t=0h13m36s)

* While our newer Span-writing methods return the number of elements written (when there's not a better purpose for the return value) this has symmetry with the current Span methods.

```C#
namespace System
{
    public sealed class String
    {
        ...
        public void CopyTo(int sourceIndex, char[] destination, int destinationIndex, int count);
+       public void CopyTo(Span<char> destination);
+       public bool TryCopyTo(Span<char> destination);
    }
}
```
## Consider deprecating or annotating a few S.S.C.X509Certificates members

**Approved** | [#runtime/47977](https://github.com/dotnet/runtime/issues/47977#issuecomment-848081543) | [Video](https://www.youtube.com/watch?v=kO1HyNmi7ww&t=0h26m6s)

* The obsoletions should use the next available SYSLIB number
* The SupportedOSPlatform attributes were moved from the property to just the property setter (so the getters are still allowed on all platforms, since they don't throw).

```diff
public class X509Certificate2
{
    public bool Archived {
       get;
+   [SupportedOSPlatform("windows")]
       set;
 }

    public string FriendlyName
     {
         get;
+   [SupportedOSPlatform("windows")]
         set;
     }

+   [Obsolete("This is no longer the recommended way of retrieving the private key. Use the appropriate GetPrivateKey or CopyWithPrivateKey method instead.")]
    public AsymmetricAlgorithm PrivateKey { get; set; }

+   [Obsolete("X509Certificate2 is immutable on this platform. Use a different constructor overload instead.")]
    public X509Certificate2();

+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(byte[] rawData);
+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(byte[] rawData, SecureString? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(byte[] rawData, string? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(string fileName);
+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(string fileName, System.Security.SecureString? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate2 is immutable on this platform. Use the equivalent constructor instead.")]
    public override void Import(string fileName, string? password, X509KeyStorageFlags keyStorageFlags);
}

public class X509Certificate
{
+   [Obsolete("X509Certificate is immutable on this platform. Use a different constructor overload instead.")]
    public X509Certificate();

+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(byte[] rawData);
+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(byte[] rawData, SecureString? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(byte[] rawData, string? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(string fileName);
+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(string fileName, System.Security.SecureString? password, X509KeyStorageFlags keyStorageFlags);
+   [Obsolete("X509Certificate is immutable on this platform. Use the equivalent constructor on X509Certificate2 instead.")]
    public void Import(string fileName, string? password, X509KeyStorageFlags keyStorageFlags);
}

public class PublicKey
{
+   [Obsolete("The key property is no longer recommended for use. Use the appropriate GetPublicKey method instead.")]
    public AsymmetricAlgorithm Key { get; }
}
```

## Add PipeWriter.UnflushedBytes

**Approved** | [#runtime/48913](https://github.com/dotnet/runtime/issues/48913#issuecomment-848161253) | [Video](https://www.youtube.com/watch?v=kO1HyNmi7ww&t=0h37m2s)

* The discussion favored this being a Try method to better communicate to the caller (and code reviewer) that the implementation might not have an answer; then circled back and split the Nullable into a virtual Can property and a virtual throwing property.

```C#
namespace System.IO.Pipelines
{
    partial class PipeWriter 
    {
         public virtual bool CanGetUnflushedBytes => false;
         public virtual long UnflushedBytes => throw new NotImplementedException(some message here);
    }
}
```
## Add blittable Color to System.Drawing

**NeedsWork** | [#runtime/48615](https://github.com/dotnet/runtime/issues/48615#issuecomment-848191829) | [Video](https://www.youtube.com/watch?v=kO1HyNmi7ww&t=1h47m30s)

It seems like there may be scenarios, but we should reconcile the layering (above or below Color) and determine if we need this, or a companion, for MAUI.
