# API Review 08/17/2021

## Pass interpolated string handlers by ref

**Approved** | [#runtime/57538](https://github.com/dotnet/runtime/issues/57538#issuecomment-900481517) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=0h0m0s)

Looks good as proposed.  Hopefully we'll be diligent in the future at always using pass-by-ref when the handler is a struct.
## Add BinderOptions.BindSingleElementsToArray flag

**Rejected** | [#runtime/57325](https://github.com/dotnet/runtime/issues/57325#issuecomment-900508614) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=0h8m16s)

We feel that the behavior isn't something that people really want to switch on/off, this is just for compatibility across an upgrade boundary.

Either just make the breaking change, or use an AppContext switch to provide "temporary" access to the previous behavior.  (API can be added if we're proven wrong).
## Quaternion.Zero is missing

**Approved** | [#runtime/57253](https://github.com/dotnet/runtime/issues/57253#issuecomment-900511049) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=0h46m9s)

Looks good as proposed.

```C#
namespace System.Numerics
{
     partial struct Quaternion
     {
        /// <summary>Gets a newly initialized quaternion.</summary>
        /// <value>A quaternion whose values are <c>(0, 0, 0, 0)</c>.</value>
        public static Quaternion Zero
        {
            get => new();
        }
     }
}
```
## X509Certificate2.RawDataMemory

**Approved** | [#runtime/57448](https://github.com/dotnet/runtime/issues/57448#issuecomment-900529048) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=0h50m4s)

Looks good as proposed.

There was much debate over ReadOnlyMemory/ReadOnlySpan/ImmutableArray and just making the existing property not return defensive copies.

```C#
namespace System.Security.Cryptography.X509Certificates {
    partial class X509Certificate2 : X509Certificate {
       public ReadOnlyMemory<byte> RawDataMemory { get; }
    }
}
```
## Consider adding Socket Send/ReceiveAsync overloads that elide SocketFlags argument

**Approved** | [#runtime/43934](https://github.com/dotnet/runtime/issues/43934#issuecomment-900535135) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h17m56s)

* We discussed defaulting the parameter where possible, and landed on overloading for symmetry with existing things.
* We moved everything from the extensions class to the main class as instance methods
* Please double check that on ReceiveMessageFrom it's not important that the ref SocketFlags is not used, since that could have been passing data back to the caller.
* Also, let's paint the current extensions type as EB-Never

```C#
namespace System.Net.Sockets
{
    // Existing overloads in comments
    partial class Socket
    {
    //  public Task<int> ReceiveAsync(ArraySegment<byte> buffer, SocketFlags socketFlags);
        public Task<int> ReceiveAsync(ArraySegment<byte> buffer);
        
    //  public Task<int> ReceiveAsync(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
        public Task<int> ReceiveAsync(IList<ArraySegment<byte>> buffers);
        
    //  public ValueTask<int> ReceiveAsync(Memory<byte> buffer, SocketFlags socketFlags, CancellationToken cancellationToken = default(CancellationToken));
        public ValueTask<int> ReceiveAsync(Memory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
        
    //  public Task<SocketReceiveFromResult> ReceiveFromAsync(ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
        public Task<SocketReceiveFromResult> ReceiveFromAsync(ArraySegment<byte> buffer, EndPoint remoteEndPoint);
        
    //  public Task<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
        public Task<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(ArraySegment<byte> buffer, EndPoint remoteEndPoint);
        
    //  public Task<int> SendAsync(ArraySegment<byte> buffer, SocketFlags socketFlags);
        public Task<int> SendAsync(ArraySegment<byte> buffer);
        
    //  public Task<int> SendAsync(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
        public Task<int> SendAsync(IList<ArraySegment<byte>> buffers);
        
    //  public ValueTask<int> SendAsync(ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, CancellationToken cancellationToken = default(CancellationToken));
        public ValueTask<int> SendAsync(ReadOnlyMemory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
        
    //  public Task<int> SendToAsync(ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP);
        public Task<int> SendToAsync(ArraySegment<byte> buffer, EndPoint remoteEP);

    //  public ValueTask<int> SendToAsync(ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = default);
        public ValueTask<int> SendToAsync(ReadOnlyMemory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = default);

    //  public ValueTask<SocketReceiveFromResult> ReceiveFromAsync(Memory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = default);
        public ValueTask<SocketReceiveFromResult> ReceiveFromAsync(Memory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = default);

    //  public ValueTask<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(Memory<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP, CancellationToken cancellationToken = default);
        public ValueTask<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(Memory<byte> buffer, EndPoint remoteEP, CancellationToken cancellationToken = default);


/*** DOUBLE CHECK THAT THIS ONE IS OK, SINCE IT IS NO LONGER "RETURNING" DATA ***/

    //  public int ReceiveMessageFrom(Span<byte> buffer, ref SocketFlags socketFlags, ref EndPoint remoteEP, out IPPacketInformation ipPacketInformation);
        public int ReceiveMessageFrom(Span<byte> buffer, ref EndPoint remoteEP, out IPPacketInformation ipPacketInformation);
    }

    [EditorBrowsable(EditorBrowsableState.Never)]
    partial class SocketTaskExtensions
    {
    }
}
```
## Add .Clear() method to MemoryCache

**Approved** | [#runtime/45593](https://github.com/dotnet/runtime/issues/45593#issuecomment-900541842) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h28m23s)


* The interface can't be changed, per our breaking change rules.
* The method also can't be added as a DIM, because the library targets .NET Standard 2.0
* The primary method is OK.
* Perhaps it's time to mark the interface as `[Obsolete]`?  Something to think about.

```C#
namespace Microsoft.Extensions.Caching.Memory
{
    partial class MemoryCache
    {
        public void Clear();
    }
}
```
## Proposal: AppContext.SetData

**Approved** | [#runtime/47922](https://github.com/dotnet/runtime/issues/47922#issuecomment-900544874) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h37m35s)

Looks good as proposed, for symmetry.

```C#
namespace System
{
    public static partial class AppContext
    {
        public static void SetData(string name, object? data);
    }
}
```
## Add DiagnosticSource.Write<T> API to assist with trimming

**Approved** | [#runtime/50454](https://github.com/dotnet/runtime/issues/50454#issuecomment-900552943) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h42m3s)

These new overloads don't quite solve the problem (recursive preservation, polymorphism), but it's an improvement at low cost.

```diff
namespace System.Diagnostics
{
    public abstract class DiagnosticSource
    {
        [RequiresUnreferencedCode]
        public abstract void Write(string name, object? value);

+       [RequiresUnreferencedCode]
+       public void Write<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] T>(string name, T? value);

        [RequiresUnreferencedCode]
        public Activity StartActivity(Activity activity, object? args);

+       [RequiresUnreferencedCode]
+       public Activity StartActivity<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] T>(Activity activity, T? args);

        [RequiresUnreferencedCode]
        public void StopActivity(Activity activity, object? args);

+       [RequiresUnreferencedCode]
+       public void StopActivity<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] T>(Activity activity, T? args);
     }
}
```
## Make DependencyModel.Dependency readonly and implement IEquatable<>

**Approved** | [#runtime/56082](https://github.com/dotnet/runtime/issues/56082#issuecomment-900554070) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h54m9s)

Looks good as proposed.

```diff
namespace Microsoft.Extensions.DependencyModel
{
-    public struct Dependency {}
+    public readonly struct Dependency : IEquatable<Dependency> {}
}
```
## Introduce pause intrinsics in order to support spin wait loop indication

**Approved** | [#runtime/53532](https://github.com/dotnet/runtime/issues/53532#issuecomment-900555431) | [Video](https://www.youtube.com/watch?v=ztrfSfgXjFU&t=1h55m46s)

Looks good as proposed.


```diff
{
    public abstract class ArmBase 
    {
+        public static void Yield();
    }
 }

    public abstract partial class X86Base 
    {
+        public static void Pause();
    }
 }
```
