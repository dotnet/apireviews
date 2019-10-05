# Quick Reviews 12/18/2018

## Native Library Resolve Event

**Approved** | [#corefx/32850](https://github.com/dotnet/corefx/issues/32850) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=0h0m0s)

## Native Library Loader API

**Approved** | [#corefx/32015](https://github.com/dotnet/corefx/issues/32015#issuecomment-448324606) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=0h7m56s)

This is the shape we agreed on:

* We centrlized the APIs on a new type `NativeLibrary` as opposed to `Marshal` because it helps the developer using these APIs as it groups them better. `Marshal` already has a ton of APIs.
* We renamed `RegisterDllImportResolver` to `SetDllImportResolver`. Register implies having an unregister and having a chain. `Set` makes it clear there is a single value and `null` will clear it.
* We replaced the func of `SetDllImportResolver` with a named delegate so that we can name the parameters.
* Open item: we need to clarify whether the `DllImportSearchPath?` argument to `DllImportResolver` is different from any other existing enum value. If not, we should make it non-nullable.


```C#
namespace System.Runtime.InteropServices
{
    public delegate IntPtr DllImportResolver(string libraryName,
                                             Assembly assembly,
                                             DllImportSearchPath? searchPath);

    public static partial class NativeLibrary
    {
        // Typical or for dependencies which must be there or failure should happen.

        public static IntPtr Load(string libraryPath);
        public static IntPtr Load(string libraryName,
                                  Assembly assembly,
                                  DllImportSearchPath? searchPath);

        // For fast probing scenarios:

        public static bool TryLoad(string libraryPath,
                                   out IntPtr handle);
        public static bool TryLoad(string libraryName,
                                   Assembly assembly,
                                   DllImportSearchPath? searchPath,
                                   out IntPtr handle);

        public static void Free(IntPtr handle);

        public static IntPtr GetExport(IntPtr handle, string name);
        public static bool TryGetExport(IntPtr handle, string name, out IntPtr address);

        public static bool SetDllImportResolver(Assembly assembly, DllImportResolver resolver);
    }
}
```
## MatchFailureException

**Approved** | [#corefx/33284](https://github.com/dotnet/corefx/issues/33284#issuecomment-448332714) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=0h30m25s)

* There is a possibility for a breaking change: library author ships a library for 3.0 and then later downgrades to 2.0, which is is expected to be transparent to consumer. Now a different exception exception is thrown. Seems remote though.
* We don't expect any user code to specifically handle this exception (outside of implicit catch-all style handlers). We should move this to `System.Runtime.CompilerServices`.
* We should have a less generic name, for instance `SwitchExpressionException`.

```C#
namespace System.Runtime.CompilerServices
{
    /// <summary>
    /// Indicates that a switch expression that was non-exhaustive failed to match its input
    /// at runtime, e.g. in the C# 8 expression <code>3 switch { 4 => 5 }</code>.
    /// The exception optionally contains an object representing the unmatched value.
    /// </summary>
    [System.Runtime.InteropServices.ComVisible(true)]
    [Serializable]
    public sealed class SwitchExpressionException : InvalidOperationException
    {
        public SwitchExpressionException();
        public SwitchExpressionException(object unmatchedValue);
        public object UnmatchedValue { get; }
        [System.Security.SecurityCritical]
        public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
}
```
## Add API for resolving component dependencies

**Approved** | [#corefx/33165](https://github.com/dotnet/corefx/issues/33165#issuecomment-448337807) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=0h54m47s)

* Should we offer a resolver that represents the "global" context? `AssemblyLoadContext` kind of does that, but it doesn't talk in paths.
* The term component seems a bit ill-defined. `AssemblyDependencyResolver` seems to make sense, given that this type is for resolving dependencies of assemblies (managed or unamanged). The only caveat is that you need to pass the path to the assembly that has the corresponding `<assembly>.deps.json` file, i.e. the main assembly or the entry point of the plug-in.

```C#
namespace System.Runtime.Loader
{
    public sealed class AssemblyDependencyResolver
    {
        // Should this API exist?
        // public static AssemblyDependencyResolver GlobalResolver { get; }

        public AssemblyDependencyResolver(string assemblyPath);

        public string ResolveAssemblyToPath(AssemblyName assemblyName);
        public string ResolveUnmanagedDllToPath(string unmanagedDllName);
    }
}
```
## Setting/Getting HighDpiMode in WinForms Core Apps

**Approved** | [#winforms/135](https://github.com/dotnet/winforms/issues/135#issuecomment-448340808) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=1h11m16s)

* It's a bit odd to use a method; one would expect a regular property setter.
* However, after reading the text it seems that's because the method might ignore the value. Presumably the method returns `false` in this case, as well as in your case (a)?
* Thus, the shape looks good as proposed.
## DataObject.SetAudio should be exposed with a new overload taking Span<byte>

**Rejected** | [#winforms/218](https://github.com/dotnet/winforms/issues/218#issuecomment-448343258) | [Video](https://www.youtube.com/watch?v=iQMNIeVoAdY&t=1h20m58s)

We aren't experts on `IDataObject` but what is the benefit of using `Span<T>` here? The `Stream` API allows you to slice the data already, and it seems the implementation pushes the state into the heap.

For this scenario `Span<T>` doesn't make sense; at the very minimum it would need to be `Memory<T>` (because `Span<T>` cannot be stored on the heap). However, this seems to complicate the API surface more than it helps.

If slicing the array would be common, I suggest we instead add an overload that simply accepts two ints, or an `ArraySegment<T>`.
