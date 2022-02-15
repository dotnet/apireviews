# API Review 02/15/2022

## Augment Regex extensibility point for better perf and span-based matching

**Approved** | [#runtime/59629](https://github.com/dotnet/runtime/issues/59629#issuecomment-1040624509)

Looks good as proposed

```diff
namespace System.Text.RegularExpressions
{
    public class RegexRunner
    {
-        protected abstract bool FindFirstChar();
-        protected abstract void Go();
-        protected abstract void InitTrackCount();
+        protected virtual bool FindFirstChar() => throw new NotImplementedException(); //default implementation
+        protected virtual void Go() => throw new NotImplementedException(); //default implementation
+        protected virtual void InitTrackCount() => return 0; //default implementation

+        protected virtual void Scan(ReadOnlySpan<char> text);

    }
    public class Regex
    {
+        public bool IsMatch(ReadOnlySpan<char> input);
+        public static bool IsMatch(ReadOnlySpan<char> input, string pattern);
+        public static bool IsMatch(ReadOnlySpan<char> input, string pattern, RegexOptions options);
+        public static bool IsMatch(ReadOnlySpan<char> input, string pattern, RegexOptions options, TimeSpan matchTimeout);
    }
}
```
## Samplers should be allowed to modify tracestate

**Approved** | [#runtime/63652](https://github.com/dotnet/runtime/issues/63652#issuecomment-1040656160)

If the new property is declared as get+init then the C# `with` syntax will allow for the easy copy-and-set, which can be written back through the ref to accomplish the current scenario.

```diff
namespace System.Diagnostics
{
    public readonly struct ActivityCreationOptions<T>
    {
+        public string? TraceState { get; init; }
    }
}
```

```C#
    listener.Sample = (ref ActivityCreationOptions<ActivityContext> activityOptions) =>
    {
        activityOptions = activityOptions with { TraceState = "rojo=00f067aa0ba902b7" };
        return ActivitySamplingResult.AllDataAndRecorded;
    };
```
## LibraryImportAttribute for p/invoke source generation

**Approved** | [#runtime/46822](https://github.com/dotnet/runtime/issues/46822#issuecomment-1040703073)

We renamed the MarshalStringsUsing property to StringMarshallingCustomType, and renamed the 0 value of StringMarshalling from `None` to `Custom` to better describe the relationship of the two properties.  Otherwise, looks good as proposed.

```C#
namespace System.Runtime.InteropServices
{
    /// <summary>
    /// Specifies how strings should be marshalled
    /// </summary>
    public enum StringMarshalling
    {
        Custom = 0,
        Utf8,   // UTF-8
        Utf16,  // UTF-16, machine-endian
    }

    /// <summary>
    /// Attribute used to indicate a Source Generator should create a function for marshaling
    /// arguments instead of relying on the CLR to generate an IL Stub at runtime.
    /// </summary>
    [AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
    public sealed class LibraryImportAttribute : Attribute
    {
        /// <summary>
        /// Constructor.
        /// </summary>
        /// <param name="libraryName">Name of the library containing the import</param>
        public LibraryImportAttribute(string libraryName);

        /// <summary>
        /// Library to load.
        /// </summary>
        public string LibraryName { get; }

        /// <summary>
        /// Indicates the name or ordinal of the entry point to be called.
        /// </summary>
        /// <see cref="System.Runtime.InteropServices.DllImportAttribute.EntryPoint"/>
        public string? EntryPoint { get; set; }

        /// <summary>
        /// Indicates how to marshal string parameters to the method.
        /// </summary>
        /// <remarks>
        /// If this field is specified, <see cref="StringMarshallingCustomType" /> must not be specified.
        /// </remarks>
        public StringMarshalling StringMarshalling { get; set; }

        /// <summary>
        /// Indicates how to marshal string parameters to the method.
        /// </summary>
        /// <remarks>
        /// If this field is specified, <see cref="StringMarshalling" /> must not be specified.
        /// The type should be one that conforms to usage with the attributes:
        /// <see cref="System.Runtime.InteropServices.MarshalUsingAttribute"/>
        /// <see cref="System.Runtime.InteropServices.NativeMarshallingAttribute"/>
        /// </remarks>
        public Type? StringMarshallingCustomType { get; set; }

        /// <summary>
        /// Indicates whether the callee sents an error (SetLastError on Windows or errorno
        /// on other platforms) before returning from the attributed method.
        /// </summary>
        /// <see cref="System.Runtime.InteropServices.DllImportAttribute.SetLastError"/>
        public bool SetLastError { get; set; }
    }
}
```
## Attributes and Analyzer Diagnostics to support custom struct marshalling in the DllImport Source Generator

**NeedsWork** | [#runtime/46838](https://github.com/dotnet/runtime/issues/46838#issuecomment-1040749264)

We started discussing this, and ran out of time.

The main points:
* Some of the behavior that's described in the shape of a Marshaller type feels like it could/should be described on an attribute applied to the Marshaller (attribute TBD).
* The collection marshaller and the item marshaller seem to have gotten out of sync on some of the names (when they have the same concept), and they should align one way or the other.

