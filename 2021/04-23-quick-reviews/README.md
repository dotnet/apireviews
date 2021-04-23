# Quick Reviews 04/23/2021

## Add DynamicallyAccessedMemberTypes.Interfaces enum value

**Approved** | [#runtime/51487](https://github.com/dotnet/runtime/issues/51487#issuecomment-825798995) | [Video](https://www.youtube.com/watch?v=CL7PuoLYiGM&t=0h0m0s)

* There was some discussion that "Interfaces" was perhaps not the best name, but we couldn't come up with a better one, and since the scope is already "members" we felt it was OK.
```diff
namespace System.Diagnostics.CodeAnalysis
{
    enum DynamicallyAccessedMemberTypes
    {
        None = 0,
        PublicParameterlessConstructor = 0x0001,
        PublicConstructors = 0x0002 | PublicParameterlessConstructor,
        NonPublicConstructors = 0x0004,
        PublicMethods = 0x0008,
        NonPublicMethods = 0x0010,
        PublicFields = 0x0020,
        NonPublicFields = 0x0040,
        PublicNestedTypes = 0x0080,
        NonPublicNestedTypes = 0x0100,
        PublicProperties = 0x0200,
        NonPublicProperties = 0x0400,
        PublicEvents = 0x0800,
        NonPublicEvents = 0x1000,
+       Interfaces = 0x2000,
        All = ~None
    }
}
```
## Writable DOM and dynamic support for 6.0

**Approved** | [#designs/163](https://github.com/dotnet/designs/pull/163) | [Video](https://www.youtube.com/watch?v=CL7PuoLYiGM&t=0h8m8s)

## Expose top-level nullability information from reflection

**NeedsWork** | [#runtime/29723](https://github.com/dotnet/runtime/issues/29723#issuecomment-825819967) | [Video](https://www.youtube.com/watch?v=CL7PuoLYiGM&t=0h18m6s)

API Review notes:

* Consider splitting State into the "read" and "write" states, to allow for straightforward expression of `public string? Foo { get; [DisallowNull] set; }`
* Does NullableState need something like "ItsComplicated" for [MaybeNullWhen] and other state-based things?
* Should there be a method to get the nullability related custom attributes associated with the context?
* NullabilityInfo.GenericTypeArguments returns an array, so it's either cloning or is mutable.  Consider ReadOnlyCollection, or making it a method
## Make CreateScope return ServiceScope that also implements IAsyncDisposable

**Approved** | [#runtime/43970](https://github.com/dotnet/runtime/issues/43970#issuecomment-825826754) | [Video](https://www.youtube.com/watch?v=CL7PuoLYiGM&t=0h44m6s)

* We renamed CreateAsyncServiceScope to CreateAsyncScope to better match the existing name
* We moved it into the extension type for the existing method to avoid making a new type

```C#
public partial class ServiceProviderServiceExtensions
{
    public static AsyncServiceScope CreateAsyncScope(this IServiceProvider serviceProvider)
    {
        return new AsyncServiceScope(serviceProvider.CreateScope());
    }
}

public struct AsyncServiceScope : IServiceScope, IAsyncDisposable
{
    private readonly IServiceScope _serviceScope;

    public AsyncServiceScope(IServiceScope serviceScope)
    {
        _serviceScope = serviceScope;
    }

    public IServiceProvider ServiceProvider => _serviceScope.ServiceProvider;

    public void Dispose()
    {
        _serviceScope.Dispose();
    }

    public ValueTask DisposeAsync()
    {
        if (_serviceScope is IAsyncDisposable ad)
        {
            return ad.DisposeAsync();
        }
        _serviceScope.Dispose();
        return ValueTask.CompletedTask;
    }
}
```
## Make improvements to signal handling on .NET applications

**Approved** | [#runtime/50527](https://github.com/dotnet/runtime/issues/50527#issuecomment-825858901) | [Video](https://www.youtube.com/watch?v=CL7PuoLYiGM&t=0h55m41s)

* "Signal" is a very generic word, how about "PosixSignal"?
* SignalHandlerRegistration should be a class instead of a struct
* SignalRegistration instead of SignalHandlerRegistration
* Use negative numbers in the PosixSignal enum and handle translation in the PAL
* Add PosixSignal enum members for all the ones we want to support, which should not include ones that we know will break the .NET runtime (SIGIO?)
* We sealed the types
* We added a constructor for PosixSignalContext for testability

```C#
namespace System.Runtime.InteropServices
{
     public enum PosixSignal
     {
         SIGHUP = -1,
         SIGINT = -2,
         SIGQUIT = -3,
         SIGTERM = -4,
         SIGCHLD = -5,
         ...
     }

     public sealed class PosixSignalContext
     {
         public PosixSignal Signal { get; }
         public bool Cancel { get; set; }

         public PosixSignalContext(PosixSignal signal) { }
     }

     public sealed class PosixSignalRegistration : IDisposable
     {
         private PosixSignalRegistration() { }

         public static PosixSignalRegistration Create(PosixSignal signal, Action<PosixSignalContext> handler);
     }
}
```

