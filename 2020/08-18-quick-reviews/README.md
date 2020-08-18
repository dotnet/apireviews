# Quick Reviews 08/18/2020

## add GetRequiredSection on Configuration (raise exception if missing section) 

**Approved** | [#runtime/40978](https://github.com/dotnet/runtime/issues/40978#issuecomment-675639726) | [Video](https://www.youtube.com/watch?v=mM3_EF-E5H8&t=0h0m0s)

* We should make this an extension method, yes, it would make more sense on `IConfiguration`, but that's not a strong reason enough to perform an API breaking change.
* The `InvalidOperationException` seems like a good choice
* The implementation should probably use the `ConfigurationExtensions.Exists()` instead of using a custom check

```C#
namespace Microsoft.Extensions.Configuration
{
    public partial class ConfigurationExtensions
    {
        public static IConfigurationSection GetRequiredSection(this IConfiguration configuration, string key);
    }
}
```
## get full path of current process executable

**Approved** | [#runtime/40862](https://github.com/dotnet/runtime/issues/40862#issuecomment-675664341) | [Video](https://www.youtube.com/watch?v=mM3_EF-E5H8&t=0h11m21s)

* Similar to `ApplicationProcessPath` they should probably all be marked nullable
    - We decided to make `AppContext` non-nullable and return an empty string, but we could normalize that here.
* We considered putting the APIs on `AppContext` but we don't believe that's a type people should be looking it because it's the platform's quirking mechanism
* `AppEntryPointPath` returns a non-existing path for single file which makes it ill-defined (see: https://github.com/dotnet/runtime/issues/40874)
    - The API would be useful if it could be passed to the assembly load APIs, which doesn't seem to work
    - The only other scenario would getting the `FileVersionInfo` off of the application

```C#
namespace System
{
    public partial class Environment
    {
        // Returns the path to the file that launched the process. For framework dependent apps
        // this will return the path to dotnet.exe. For environment where this concept doesn't
        // exist, for example WebAssembly, the method will return null. 
        public string? AppProcessPath => Process.GetCurrentProcess().MainModule.FileName;

        // Returns the path to the file that contains the `Main` method.
        // Excluded. See comment above.
        // public string? AppEntryPointPath => GetCommandLineArgs()[0];

        // Returns the directory path of your application. Same as AppContext.BaseDirectory.
        public string? AppBaseDirectory => AppContext.BaseDirectory;
    }
}
```

