# Quick Reviews 3/26/2019

## API proposal AssemblyLoadContext.ActiveForContextSensitiveReflection

**Approved** | [#corefx/36236](https://github.com/dotnet/corefx/issues/36236#issuecomment-476796579) | [Video](https://www.youtube.com/watch?v=SNc-nABLZws&t=-17h-2m-8s)

```C#
namespace System.Runtime.Loader
{
    public partial class AssemblyLoadContext
    {
        public static AssemblyLoadContext CurrentContextualReflectionContext { get; }

        public ContextualReflectionScope EnterContextualReflection();
        static public ContextualReflectionScope EnterContextualReflection(Assembly activating);

        [EditorBrowsable(Never)]
        public struct ContextualReflectionScope : IDisposable
        {
        }
    }
}
```
## AssemblyLoadContext .NET Core 3.0 improvements

**Approved** | [#corefx/34791](https://github.com/dotnet/corefx/issues/34791#issuecomment-476796715) | [Video](https://www.youtube.com/watch?v=SNc-nABLZws&t=0h42m56s)

* Replace `AssemblyLoadContext.Contexts` with `AssemblyLoadContext.All` to make `foreach` easier to read
* Other than that, looks good as proposed

