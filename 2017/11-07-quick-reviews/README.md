# Quick Reviews 11/7/2017

## Expose RuntimeWrappedException constructor

**Approved** | [#corefx/24946](https://github.com/dotnet/corefx/issues/24946#issuecomment-342573013) | [Video](https://www.youtube.com/watch?v=HnKmLJcEM74&t=0h0m0s)

Looks good as proposed.
## Provide IEnumerable<T> support for Memory<T>

**Approved** | [#corefx/24854](https://github.com/dotnet/corefx/issues/24854#issuecomment-342577441) | [Video](https://www.youtube.com/watch?v=HnKmLJcEM74&t=0h4m12s)

Looks good as proposed. `MemoryExtensions` doesn't exist yet but `SpanExtensions` will be renamed to it.
## Add String support to ReadOnlyMemory<char>

**Approved** | [#corefx/25085](https://github.com/dotnet/corefx/issues/25085#issuecomment-342585466) | [Video](https://www.youtube.com/watch?v=HnKmLJcEM74&t=0h18m47s)

Based on the discussion with @stephentoub we concluded that we should merge the two types:

```C#
namespace System
{
    public static class MemoryExtensions
    {
        // Existing class containing AsReadOnlySpan(this string)
        public static ReadOnlyMemory<char> AsReadOnlyMemory(this string text);

        // We already have ReadOnlyMemory<T>.TryGetArray(out ArraySegment<T> segment)
        public static bool TryGetString(this ReadOnlyMemory<char> readOnlyMemory, out string value, out int offset, out int count);
    }
}
```
## Add SpanExtensions.LastIndexOf

**Approved** | [#corefx/24839](https://github.com/dotnet/corefx/issues/24839#issuecomment-342587703) | [Video](https://www.youtube.com/watch?v=HnKmLJcEM74&t=0h47m24s)

Looks good as proposed
## Additional API for DbProviderFactories in .NET Core

**Approved** | [#corefx/20903](https://github.com/dotnet/corefx/issues/20903#issuecomment-342605350) | [Video](https://www.youtube.com/watch?v=HnKmLJcEM74&t=0h53m35s)

Looks good with some minor tweaks:


```C#
public static class DbProviderFactories
{
    // exiting members
    
    public static DbProviderFactory GetFactory(string providerInvariantName); 
    public static DbProviderFactory GetFactory(DataRow providerRow); 
    public static DbProviderFactory GetFactory(DbConnection connection); 
    public static DataTable GetFactoryClasses(); 

    // new members

    /// <summary>
    /// Registers a provider factory using the assembly qualified name of the factory and an 
    /// invariant name
    /// </summary>
    public static void RegisterFactory(string providerInvariantName, string factoryTypeAssemblyQualifiedName);

    /// <summary>
    /// Registers a provider factory using the provider factory type and an invariant name
    /// </summary>
    public static void RegisterFactory(string providerInvariantName, Type factoryType);

    /// <summary>
    /// Register a provider factory using the provider factory instance and 
    /// an invariant name
    /// </summary>
    public static void RegisterFactory(string providerInvariantName, DbProviderFactory factory);

    /// <summary>
    /// Returns the provider factory instance if one is registered for the given invariant name
    /// </summary>
    public static bool TryGetFactory(string providerInvariantName, out DbProviderFactory factory);

    /// <summary>
    /// Removes the provider factory registration for the given invariant name  
    /// </summary>
    public static bool UnregisterFactory(string providerInvariantName);

    /// <summary>
    /// Returns the invariant names for all the factories registered
    /// </summary>
    public static IEnumerable<string> GetProviderInvariantNames();
}
```
