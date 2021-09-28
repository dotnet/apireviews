# API Review 09/28/2021

## Custom attribute for Pre-compile AOP

**NeedsWork** | [#runtime/59454](https://github.com/dotnet/runtime/issues/59454) | [Video](https://www.youtube.com/watch?v=modnFoA7Qts&t=0h0m0s)

## Obsolete Regex.CompileToAssembly

**Approved** | [#runtime/50535](https://github.com/dotnet/runtime/issues/50535#issuecomment-929507983) | [Video](https://www.youtube.com/watch?v=modnFoA7Qts&t=0h7m54s)

* Looks good
    - We should add a diagnostic ID for all these obsoletions.
    - Adjust message as needed to be consistent
* Should we also obsolete the other APIs that persist an assembly to disk, such as
    -  `AssemblyBuilder.Save`

```C#
namespace System.Text.RegularExpressions
{
    public partial class Regex
    {
        [Obsolete("This API isn't supported anymore. Please use source generated regexes instead.", DiagnosticId = "TBD")]
        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname);
        [Obsolete("This API isn't supported anymore. Please use source generated regexes instead.", DiagnosticId = "TBD")]
        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname, CustomAttributeBuilder[]? attributes);
        [Obsolete("This API isn't supported anymore. Please use source generated regexes instead.", DiagnosticId = "TBD")]
        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname, CustomAttributeBuilder[]? attributes, string? resourceFile);
    }
    [Obsolete("This API isn't supported anymore. Please use source generated regexes instead.", DiagnosticId = "TBD")]
    public partial class RegexCompilationInfo
    {

    }
}

```
## Add new ObjectDisposedException constructor overload

**Approved** | [#runtime/51700](https://github.com/dotnet/runtime/issues/51700#issuecomment-929511062) | [Video](https://www.youtube.com/watch?v=modnFoA7Qts&t=0h11m11s)

* Makese sense. It seems we should establish as a general pattern for throw helpers.

```C#
namespace System
{
    public partial class ObjectDisposedException
    {
        public static void ThrowIf([DoesNotReturnIf(true)] bool condition, object instance);
        public static void ThrowIf([DoesNotReturnIf(true)] bool condition, Type type);
    }
}
```
## Add RemoveAt() and Insert() to IList by using Index struct

**NeedsWork** | [#runtime/58426](https://github.com/dotnet/runtime/issues/58426#issuecomment-929521369) | [Video](https://www.youtube.com/watch?v=modnFoA7Qts&t=0h15m29s)

* The concept makes sense, but we should avoid forcing consumers through interface dispatch
    - At least for low-overhead types such as `List<T>` we should consider adding matching instance methods
    - Also, are these really all the ones that are missing?
    - Alternatively, could this be done by the compiler?

```C#

namespace System.Collections.Generic
{
    public static  class CollectionExtensions
    {
        public static void RemoveAt<T>(this IList<T> list, Index index);
        public static void Insert<T>(this IList<T> list, Index index, T data);
    }
}
```
