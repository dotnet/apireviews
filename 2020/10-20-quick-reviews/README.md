# Quick Reviews 10/20/2020

## Add System.BinaryData

**Approved** | [#runtime/41686](https://github.com/dotnet/runtime/issues/41686#issuecomment-713056163) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=0h0m0s)

* The type isn't meant to unify different representations
* Rather, it's a helper to make it easier for users to convert data
* Thus, it's not meant as high-performance exchange type, like span.
* It's for "user data", that is, the contract for the API is "give form blob of binary data". It's not for protocol-like APIs where the underlying encoding is part of the API contract, e.g. a REST API.
    - This means the API is meant for things like Azure Blob Storage, Azure Queuing, but not `HttpClient`
* We should consider making the type a class. It's unlike to have perf implications (given that constructing it generally requires copying)
    - This also gives us flexibility in the future, e.g. sub-classing
    - Not much value in sealing (we only give up not being able to add abstracts later)
* The methods that perform JSON serialization/deserialization should take JSON options
    - We should make this the second parameter so we can add new serializers where type remains optional
* We can remove the `FromObjectAsync` and `ToObjectAsync` methods, they are hold over from the serialization abstraction which had async implementations
* We should remove the constructor and factory that takes a span, because it copies and it collides wit the `ReadOnlyMemory<T>`
* We should add an implicit conversion to span (guidelines say that one shouldn't overload between them, so it seems generally fine)
* We should consider hiding `Equals()`, `GetHashCode()`
* The constructors that take `byte[]` and `ReadOnlyMemory<byte>` will not copy; they just wrap the payload. Mutations will be observed.

```C#
namespace System
{
    public class BinaryData
    {
        public BinaryData(byte[] data);
        public BinaryData(object jsonSerializable, JsonSerializerOptions options = default, Type? type = null);
        public BinaryData(ReadOnlyMemory<byte> data);
        public BinaryData(string data);
        public static BinaryData FromBytes(ReadOnlyMemory<byte> data);
        public static BinaryData FromBytes(byte[] data);
        public static BinaryData FromObjectAsJson<T>(T jsonSerializable, JsonSerializerOptions options = default, CancellationToken cancellationToken = default);
        public static BinaryData FromStream(Stream stream);
        public static Task<BinaryData> FromStreamAsync(Stream stream, CancellationToken cancellationToken = default);
        public static BinaryData FromString(string data);
        public static implicit operator ReadOnlyMemory<byte>(BinaryData data);
        public static implicit operator ReadOnlySpan<byte>(BinaryData data);
        public ReadOnlyMemory<byte> ToBytes();
        public T ToObjectFromJson<T>(JsonSerializerOptions options = default, CancellationToken cancellationToken = default);
        public Stream ToStream();
        [EditorBrowsable(EditorBrowsableState.Never)]
        public override bool Equals(object? obj);
        [EditorBrowsable(EditorBrowsableState.Never)]
        public override int GetHashCode();
        public override string ToString();
    }
}
```

## [Arm64] MultiplyHigh

**Approved** | [#runtime/43106](https://github.com/dotnet/runtime/issues/43106#issuecomment-713059473) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h31m50s)

Looks good as proposed:

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract class ArmBase
    {
        public abstract class Arm64
        {
            public static long MultiplyHigh(long left, long right);
            public static ulong MultiplyHigh(ulong left, ulong right);
        }
    }
}
```
## Add analyzer for Environment.ProcessPath 

**Approved** | [#runtime/42948](https://github.com/dotnet/runtime/issues/42948#issuecomment-713060840) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h36m52s)

Makes sense as proposed
## Add analyzer for Environment.CurrentManagedThreadId

**Approved** | [#runtime/43257](https://github.com/dotnet/runtime/issues/43257#issuecomment-713061245) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h38m35s)

Makes sense. Should use the same diagnostic ID as #42948 (we already have one for `ProcessId`, we should reuse that).
## Missing `System.Net.Sockets.TcpClient.Connect(IPEndPoint remoteEP)` overload.

**Approved** | [#runtime/43111](https://github.com/dotnet/runtime/issues/43111#issuecomment-713067442) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h39m16s)

* Looks good as proposed

```C#
namespace System.Net.Sockets
{
    public class TcpClient : IDisposable
    {
        //public void Connect(IPAddress address, int port);
        //public void Connect(IPAddress[] ipAddresses, int port);
        //public Task Connect(IPEndPoint remoteEP);
        //public void Connect(string hostname, int port);

        //public Task ConnectAsync(IPAddress address, int port);
        //public Task ConnectAsync(IPAddress[] addresses, int port);
        //public Task ConnectAsync(string host, int port);
        public Task ConnectAsync(IPEndPoint remoteEP);

        //public ValueTask ConnectAsync(IPAddress address, int port, CancellationToken cancellationToken);
        //public ValueTask ConnectAsync(IPAddress[] addresses, int port, CancellationToken cancellationToken);
        //public ValueTask ConnectAsync(string host, int port, CancellationToken cancellationToken);
        public ValueTask ConnectAsync(IPEndPoint remoteEP, CancellationToken cancellationToken);
    }
}
```
## obsolete DisablePrivateReflectionAttribute

**Approved** | [#runtime/43247](https://github.com/dotnet/runtime/issues/43247#issuecomment-713070795) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h47m13s)

* We should use a diagnostic ID. This allows us to have a help-topic specific for this issue. `SYSLIB0013` sounds yes. The topic seems to warrant that.
* Other than that, looks good.

```C#
namespace System.Runtime.CompilerServices
{
    [Obsolete("DisablePrivateReflectionAttribute has no effect in .NET Core and .NET 5.0+ applications.")]
    public partial class DisablePrivateReflectionAttribute
    {
    }
}
```
## Prefer static HashData methods over ComputeHash

**Approved** | [#runtime/40579](https://github.com/dotnet/runtime/issues/40579#issuecomment-713072863) | [Video](https://www.youtube.com/watch?v=pMasvErucTg&t=1h53m21s)

This makes sense.
