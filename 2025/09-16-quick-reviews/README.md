# API Review 09/16/2025

## MSBuild APIs for supporting multithreaded mode of operation

**Approved** | [#runtime/119465](https://github.com/dotnet/runtime/issues/119465#issuecomment-3299876858) | [Video](https://www.youtube.com/watch?v=rsjimPqjyDY&t=0h0m0s)

* `TaskExecutionContext`, sounds like a type deriving from `ExecutionContext`, so a different name might be good.
  * `TaskState`?  `TaskEnvironment`?
  * Updated the proposal to reflect TaskEnvironment as the new working name.
* The `abstract` of TaskExecutionContext doesn't seem necessary.  On any of the methods.
* Constructing an AbsolutePath from a string containing an absolute path feels cumbersome.
  * Moved `internal AbsolutePath(string)` to `internal AbsolutePath(string, bool)`, and added `public AbsolutePath(string)` which can use Path.IsPathRooted(string) and throw if the input is not.
* In discussion, it seemed like RelativePath is not really needed outside of MSBuild itself, so it has been removed from the proposal.
* Not all of the overloads of `StartProcess` seem important here.
  * StartProcess(ProcessStartInfo) is either trivial, and shouldn't exist, or overwrites CurrentDirectory and environment, which is surprising (so it probably shouldn't exist).
  * If the other two overloads are going to eliminate the need to configure ProcessStartInfo for "99%" of potential callers, then they're fine, otherwise recommend cutting them in favor of just calling GetProcessStartInfo()
* Decide what behavior you want for default(AbsolutePath).
  * Perhaps the Path property should return string.Empty.
  * Other suggestions include system root (`/` or `%SYSTEMROOT%\`), or `GetRootedProjectPathFromMSBuild()`
* For the analyzer looking at AbsolutePath, be sure to check for the implicit operator, the Path property, or the ToString() method, all are viable ways of converting the struct to a path-string.
* `[ThreadSafe]` is an overreaching name.
  * `[MSBuildThreadSafeTask]` seems like a reasonable name.
  * Consider not shipping the public copy if the expected behavior is that libraries with a high enough compile target just use the new interface.
* Instead of `ThreadSafeTask` and `IThreadSafeTask`, consider `ParallelAwareTask`, `MultiThreadableTask`, etc.

```C#
namespace Microsoft.Build.Framework;

public interface IMultiThreadableTask : ITask
{
    TaskEnvironment TaskEnvironment { get; set; }
}

public class TaskEnvironment
{ 
    public AbsolutePath CurrentDirectory { get; set; }

    public AbsolutePath GetAbsolutePath(string path);
    
    public string? GetEnvironmentVariable(string name);
    public IReadOnlyDictionary<string, string> GetEnvironmentVariables();
    public void SetEnvironmentVariable(string name, string? value);

    public ProcessStartInfo GetProcessStartInfo();
    public Process StartProcess(string fileName);
    public Process StartProcess(string fileName, IEnumerable<string> arguments);
}

public readonly struct AbsolutePath
{
    public string Path { get; }
    // Tasks authors should not be able to create AbsolutePath object without using TaskExecutionContext object.
    internal AbsolutePath(string path, bool ignoreRootedCheck) { }

    public AbsolutePath(string path); // Checks Path.IsPathRooted
    public AbsolutePath(string path, AbsolutePath basePath) { }
    public static implicit operator string(AbsolutePath path) { }
    public override string ToString() => Path;
}

[AttributeUsage(AttributeTargets.Class, AllowMultiple = false)]
public class MSBuildThreadSafeTaskAttribute : Attribute
{
    public MSBuildThreadSafeTaskAttribute() { }
}

namespace Microsoft.Build.Utilities;

public abstract class MultiThreadableTask : Task, IMultiThreadableTask
{
    public TaskEnvironment? TaskEnvironment{ get; set; }
}
```
## Hide NamedPipeClientStream ctor with isConnected argument

**Approved** | [#runtime/119671](https://github.com/dotnet/runtime/issues/119671#issuecomment-3299886114) | [Video](https://www.youtube.com/watch?v=rsjimPqjyDY&t=1h16m47s)

* Given that we can find very few callers of this constructor, and `isConnected: false` doesn't seem to make sense, Obsolete+EB-Never makes sense.
* Use the next available SYSLIB diagnostic ID.

```diff
namespace System.IO.Pipes;

public sealed partial class NamedPipeClientStream : PipeStream
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
+   [Obsolete("This constructor has been deprecated and argument bool isConnected does not have any effect. Use NamedPipeClientStream(PipeDirection direction, bool isAsync, SafePipeHandle safePipeHandle) instead.")]
    public NamedPipeClientStream(PipeDirection direction, bool isAsync, bool isConnected, SafePipeHandle safePipeHandle) : base (default(PipeDirection), default(int)) { }

+   public NamedPipeClientStream(PipeDirection direction, bool isAsync, SafePipeHandle safePipeHandle) : base(default(PipeDirection), default(int)) { }
}
```
## Support detect keyed dependency injected IDistributedCache as backend cache for DefaultHybridCache

**Approved** | [#runtime/117976](https://github.com/dotnet/runtime/issues/117976#issuecomment-3299923407) | [Video](https://www.youtube.com/watch?v=rsjimPqjyDY&t=1h19m58s)

Looks good as proposed.

There was some discussion about whether to try to collapse the optional parameters for `AddKeyedHybridCache`, and in the end decided it was consistent with the existing members on that type to just use overloading.

```csharp
namespace Microsoft.Extensions.Caching.Hybrid
{
    public partial class HybridCacheOptions
    {
        public object? DistributedCacheServiceKey { get; set; }
    }
}

namespace Microsoft.Extensions.DependencyInjection
{
    public static class HybridCacheServiceExtensions
    {
        public static IHybridCacheBuilder AddKeyedHybridCache(this IServiceCollection services, object? serviceKey);
        public static IHybridCacheBuilder AddKeyedHybridCache(this IServiceCollection services, object? serviceKey, string optionsName);
        public static IHybridCacheBuilder AddKeyedHybridCache(this IServiceCollection services, object? serviceKey, Action<HybridCacheOptions> setupAction);
        public static IHybridCacheBuilder AddKeyedHybridCache(this IServiceCollection services, object? serviceKey, string optionsName, Action<HybridCacheOptions> setupAction);
    }
}
```
## Add support for `HostApplicationBuilder` in AmbientMetadata extension

**Approved** | [#extensions/6509](https://github.com/dotnet/extensions/issues/6509#issuecomment-3299948027) | [Video](https://www.youtube.com/watch?v=rsjimPqjyDY&t=1h32m4s)

* This seems to be introducing a new pattern of fluency on IHostApplicationBuilder that is not already present.  If that's intended to be the new pattern going forward, fine, but if it's a one-off it should be non-generic, void-returning to match the rest of the libraries in Extensions.
* Otherwise, looks good as proposed.

```csharp
namespace Microsoft.Extensions.Hosting;
 
public static partial class ApplicationMetadataHostBuilderExtensions
{
    public static TBuilder UseApplicationMetadata<TBuilder>(this TBuilder builder, string sectionName = DefaultSectionName)
        where TBuilder : IHostApplicationBuilder;
}
```
