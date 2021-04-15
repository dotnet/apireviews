# Quick Reviews 04/15/2021

## Add ability to explicitly send EOF for connected Streams

**Approved** | [#runtime/43290](https://github.com/dotnet/runtime/issues/43290#issuecomment-820633348) | [Video](https://www.youtube.com/watch?v=ZyivxqIt4Zk&t=0h0m0s)

We felt that the concept really warranted introducing a new middle type to the hierarchy:

```C#
namespace System.IO
{
    public abstract class DuplexStream : Stream
    {
        // when disposed, this write-only stream will call CompleteWrites().
        // this allows compat with e.g. StreamWriter that knows nothing about shutdown.
        public Stream GetWriteOnlyStream() => throw null;
    
        public abstract void CompleteWrites();
        public abstract ValueTask CompleteWritesAsync(CancellationToken cancellationToken = default);
    
        override all the things;
    }
}

partial class NetworkStream : DuplexStream 
{
}

and other streams as needed.
```

(I edited so that NetworkStream derives from DuplexStream, not BidirectionalStream)
## Seal internal/private types

**Approved** | [#runtime/49944](https://github.com/dotnet/runtime/issues/49944#issuecomment-820636274) | [Video](https://www.youtube.com/watch?v=ZyivxqIt4Zk&t=1h10m5s)

Sounds good, assuming the general heuristics of

* Internal and private non-static, non-abstract, non-sealed classes that
* Don’t have any derived types in its containing assembly

But it needs to be opt-in.

## Add an API to reduce GC memory usage

**Approved** | [#runtime/50902](https://github.com/dotnet/runtime/issues/50902#issuecomment-820662422) | [Video](https://www.youtube.com/watch?v=ZyivxqIt4Zk&t=1h13m47s)

* We changed the int 0-9 to a double 0-1
* We moved it onto GCSettings and renamed it.
* Otherwise, looks good.

```C#
namespace System.Runtime
{
    partial class GCSettings
    {
        // NaN represents "there is no goal"
        // Setter throws for things outside the range [0, 1] union NaN
        public double MemoryOptimizationGoal { get; set; }
    }
}
```
## Extensible Calling Conventions for native callee functions

**Approved** | [#runtime/51156](https://github.com/dotnet/runtime/issues/51156#issuecomment-820672447) | [Video](https://www.youtube.com/watch?v=ZyivxqIt4Zk&t=1h55m18s)

* Should it also work on Delegates? (We feel no is OK)
* Sealed the attribute, since its for tooling
* AllowMultiple should be false
* We like CallConv more than Callee in this context
* The CallConvs member should be a property, but since UnmanagedCallersOnly has a field we went for symmetry.
* An analyzer to notice that DllImport had a convention specified and UnmanagedCallConv was also specified would be valuable.

```C#
namespace System.Runtime.InteropServices
{
     /// <summary>
     /// Provides an equivalent to <see cref="UnmanagedCallersOnlyAttribute"/> for native
     /// functions declared in .NET.
     /// </summary>
     [AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]
     public sealed class UnmanagedCallConvAttribute : Attribute
     {
         public UnmanagedCallConvAttribute() { }
 
         /// <summary>
         /// Types indicating calling conventions for the unmanaged target.
         /// </summary>
         /// <remarks>
         /// If <c>null</c>, the semantics are identical to <c>CallingConvention.Winapi</c>.
         /// </remarks>
         public Type[]? CallConvs;
     }
}
```
