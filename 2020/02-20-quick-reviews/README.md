# Quick Reviews 2/20/2020

## Add CancellationToken overloads to Socket.ConnectAsync and Socket.AcceptAsync

**Approved** | [#runtime/921](https://github.com/dotnet/runtime/issues/921#issuecomment-589234342) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=0h0m0s)

* We should file an issue to cover the remaining APIs `ReceiveAsync`, `ReceiveFromAsync`, `ReceiveMessageFromAsync`, `SendToAsync (@scalablecory)
* Otherwise looks good

```C#
class SocketTaskExtensions
{
   public static ValueTask ConnectAsync (this Socket socket, EndPoint remoteEP, CancellationToken cancellationToken);
   public static ValueTask ConnectAsync (this Socket socket, IPAddress address, int port, CancellationToken cancellationToken);
   public static ValueTask ConnectAsync (this Socket socket, IPAddress[] addresses, int port, CancellationToken cancellationToken);
   public static ValueTask ConnectAsync (this Socket socket, string host, int port, CancellationToken cancellationToken);
   public static ValueTask<Socket> AcceptAsync (this Socket socket, CancellationToken cancellationToken);
   public static ValueTask<Socket> AcceptAsync (this Socket socket, Socket acceptSocket, CancellationToken cancellationToken);

   // existing methods:
   // public static Task ConnectAsync (this Socket socket, EndPoint remoteEP);
   // public static Task ConnectAsync (this Socket socket, IPAddress address, int port);
   // public static Task ConnectAsync (this Socket socket, IPAddress[] addresses, int port);
   // public static Task ConnectAsync (this Socket socket, string host, int port);
   // public static Task<Socket> AcceptAsync (this Socket socket);
   // public static Task<Socket> AcceptAsync (this Socket socket, Socket acceptSocket);
}

class Socket
{
   public static void CancelAcceptAsync(SocketAsyncEventArgs e);

   // existing methods:
   // public bool AcceptAsync (SocketAsyncEventArgs e);
   // public bool ConnectAsync (SocketAsyncEventArgs e);
   // public static void CancelConnectAsync (SocketAsyncEventArgs e);
}

class TcpClient
{
   public ValueTask ConnectAsync (string host, int port, CancellationToken cancellationToken);
   public ValueTask ConnectAsync (IPAddress address, int port, CancellationToken cancellationToken);
   public ValueTask ConnectAsync (IPAddress[] addresses, int port, CancellationToken cancellationToken);

   // existing methods:
   // public Task ConnectAsync (string host, int port);
   // public Task ConnectAsync (IPAddress address, int port);
   // public Task ConnectAsync (IPAddress[] addresses, int port);
}

class TcpListener
{
   public ValueTask<Socket> AcceptSocketAsync (CancellationToken cancellationToken);
   public ValueTask<TcpClient> AcceptTcpClientAsync (CancellationToken cancellationToken);

   // existing methods:
   // public Task<Socket> AcceptSocketAsync ();
   // public Task<TcpClient> AcceptTcpClientAsync ();
}
```
## Socket.SetRawSocketOption

**Approved** | [#runtime/865](https://github.com/dotnet/runtime/issues/865#issuecomment-589239349) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=0h11m7s)

* We considered making it `TryGetRawSocketOption` but we can't think of a case where the size of the option would change/is not known.

```C#
public partial class Socket
{
    // existing: public void SetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, byte[] optionValue);
    public void SetRawSocketOption(int optionLevel, int optionName, ReadOnlySpan<byte> optionValue);

    // existing: public byte[] GetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, int optionLength);
    public int GetRawSocketOption(int optionLevel, int optionName, Span<byte> optionValue);
}
```
## Update Encoder, EncoderParameterValueType to match GDI+

**Approved** | [#runtime/30543](https://github.com/dotnet/runtime/issues/30543#issuecomment-589242229) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=0h22m22s)

* Looks good as proposed

```C#
namespace System.Drawing.Imaging
{
    public sealed partial class Encoder
    {
        // Existing:
        // public static readonly Encoder ChrominanceTable;
        // public static readonly Encoder ColorDepth;
        // public static readonly Encoder Compression;
        // public static readonly Encoder LuminanceTable;
        // public static readonly Encoder Quality;
        // public static readonly Encoder RenderMethod;
        // public static readonly Encoder SaveFlag;
        // public static readonly Encoder ScanMethod;
        // public static readonly Encoder Transformation;
        // public static readonly Encoder Version;
        public static readonly Encoder ColorSpace;
        public static readonly Encoder ImageItems;
        public static readonly Encoder SaveAsCmyk;
    }

    public enum EncoderParameterValueType
    {
        // Existing:
        // ValueTypeByte = 1,
        // ValueTypeAscii = 2,
        // ValueTypeShort = 3,
        // ValueTypeLong = 4,
        // ValueTypeRational = 5,
        // ValueTypeLongRange = 6,
        // ValueTypeUndefined = 7,
        // ValueTypeRationalRange = 8,
        ValueTypePointer = 9,
    }
}
```
## SequenceReader<T>.TryRead overloads to read a specified number of elements

**Approved** | [#runtime/30778](https://github.com/dotnet/runtime/issues/30778#issuecomment-589257975) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=0h29m12s)

* The first API will have to allocate when `count` causes spanning blocks in the sequence. The bigger the `count`, the more likely it is to cross such a boundary. This could be changed to allow the caller to pass in a pre-allocated span or we could change the semantics to only succeed if the data is available contiguously. This API will need more design.
* The second overload is fine but we should change the name to `TryReadExact` to make it clear that returning less or more is both not possible.

```C#
public partial ref struct SequenceReader<T>
{
    bool TryReadExact(int count, out ReadOnlySequence<T> value);
}
```
## SequenceReader<T>.TryPeek overloads to read a specified number of elements from any position

**Needs Work** | [#runtime/30779](https://github.com/dotnet/runtime/issues/30779#issuecomment-589267066) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h5m23s)

* The two `TryPeek` APIs that out a span need more work because they silently allocate. Either allow for the caller to pass in a pre-allocated span or make it clear in the name that it will only return true if the data is in contiguous memory.
* The `TryPeek` overloads involving sequences look fine, except that `count` changes position due to `skip`. We should either remove the one that doesn't take `skip` (callers would have to pass in `0`) or we should change order of `skip` which might be weird.
* `TryCopyTo` seems fine as proposed, but it seems we might want to change it based on what we do for `TryPeek`.

@davidfowl looks like we should meet and discuss this.
## Add generic overloads for MethodInfo.CreateDelegate

**Approved** | [#runtime/30800](https://github.com/dotnet/runtime/issues/30800#issuecomment-589273550) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h24m38s)

* We discussed dropping the generic constraint (because it makes the new API less useful to code that is generic and not constrained, because the constraint is recent).
* We'll keep it for now and will drop it if we see issues with the constraint.

```C#
public partial class MethodInfo
{
    // Current
    public virtual Delegate CreateDelegate(Type delegateType);
    public virtual Delegate CreateDelegate(Type delegateType, object target);

    // New
    public T CreateDelegate<T>() where T : Delegate => (T)CreateDelegate(typeof(T));
    public T CreateDelegate<T>(object? target) where T : Delegate => (T)CreateDelegate(typeof(T), target);
}
```
## Add TaskCompletionSource.SetCanceled(CancellationToken)

**Approved** | [#runtime/30862](https://github.com/dotnet/runtime/issues/30862#issuecomment-589274248) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h39m39s)

* Looks good as proposed and follows the guidelines for `Try`-APIs

```C#
public class TaskCompletionSource<TResult>
{
    ...
    public void SetCanceled(CancellationToken cancellationToken);
    ...
}
```
## Sse2.StoreScalar(int*)

**Approved** | [#runtime/31179](https://github.com/dotnet/runtime/issues/31179#issuecomment-589275674) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h41m12s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Sse2 : Sse
    {
        public static unsafe void StoreScalar(int* address, Vector128<int> source);
        public static unsafe void StoreScalar(uint* address, Vector128<uint> source);
    }
}
```
## long Math.BigMul(long, long, out long)

**Approved** | [#runtime/31184](https://github.com/dotnet/runtime/issues/31184#issuecomment-589278166) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h44m34s)

* Looks good as proposed

```C#
namespace System
{
    public partial class Math
    {
        public ulong BigMul(ulong a, ulong b, out ulong low);
        public long BigMul(long a, long b, out long low);
    }
}
```
## Avx.CompareXXX convenience methods for float/double

**Approved** | [#runtime/31193](https://github.com/dotnet/runtime/issues/31193#issuecomment-589280411) | [Video](https://www.youtube.com/watch?v=DWt2Dh5L-C0&t=1h50m5s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Avx : Sse42
    {
        public static Vector256<float> CompareEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareEqual(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareGreaterThan(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareGreaterThan(Vector256<double> left,  Vector256<double> right);

        public static Vector256<float> CompareGreaterThanOrEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareGreaterThanOrEqual(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareLessThan(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareLessThan(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareLessThanOrEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareLessThanOrEqual(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareNotEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareNotEqual(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareNotGreaterThan(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareNotGreaterThan(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareNotGreaterThanOrEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareNotGreaterThanOrEqual(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareNotLessThan(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareNotLessThan(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareNotLessThanOrEqual(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareNotLessThanOrEqual(Vector256<double> left, Vector256<double> right);

        // The justification for these is questionable as the equivalent FloatComparisonMode is obvious.
        public static Vector256<float> CompareOrdered(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareOrdered(Vector256<double> left, Vector256<double> right);

        public static Vector256<float> CompareUnordered(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> CompareUnordered(Vector256<double> left, Vector256<double> right);
    }
}
```
