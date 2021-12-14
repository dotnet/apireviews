# API Review 12/14/2021

## Add support for clearing ConfigurationManager sources (was possible in version 5)

**Approved** | [#runtime/61675](https://github.com/dotnet/runtime/issues/61675#issuecomment-993852244) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=0h0m0s)

* Looks good as proposed
    - Should we also expose `IConfigurationBuilder.Properties`? What happens if `Properties` aren't cleared? Would there a benefit to add a `Clear` method that clears both?

```C#
namespace Microsoft.Extensions.Configuration
{
    public partial sealed class ConfigurationManager
    {
        // Currently explicit:
        // IList<IConfigurationSource> IConfigurationBuilder.Sources { get; }

        // Now implicit:
        public IList<IConfigurationSource> Sources { get; }
    }
}
```
## System.Diagnostics.CodeAnalysis.StringLanguageAttribute

**Approved** | [#runtime/62505](https://github.com/dotnet/runtime/issues/62505#issuecomment-993866275) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=0h11m48s)

* We think we should rename "language" to "syntax" to avoid any connotations to localization
* We should only expose the language constants as we're adding support for them

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.Parameter |
                    AttributeTargets.Field |
                    AttributeTargets.Property,
                    AllowMultiple = false,
                    Inherited = false)]
    public sealed class StringSyntaxAttribute : Attribute
    {
        public StringSyntaxAttribute(string syntax);
        public string Syntax { get; }

        public const string Regex = "regex";

        // As we're adding more support we can add new languages like:
        // public const string Xml = "xml";
        // public const string Json = "json";
    }
}
```
## Remove redundant configuration from [DllImport] declaration

**Rejected** | [#runtime/33808](https://github.com/dotnet/runtime/issues/33808#issuecomment-993877673) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=0h28m2s)

* We're not convinced that over-specification is a bad practice; these are expert-level APIs and many experts like being explicit instead of relying on defaults.
* However, there is value in P/Invoke analyzers, which, could flag when people are calling Unicode APIs (*W) but specify ANSI encoding, for instance.
* Let's close this one for now. If there are other suggestions, we can open new issues.
## Add `System.Reflection.Metadata.Ecma335.ControlFlowBuilder.Reset`.

**Approved** | [#runtime/58765](https://github.com/dotnet/runtime/issues/58765#issuecomment-993883006) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=0h43m18s)

* Makes sense, but we'd prefer the term `Clear`
* CC @tmat 

```C#
namespace System.Reflection.Metadata.Ecma355
{
     public partial sealed class ControlFlowBuilder
     {
          public void Clear();
     }
}
```
## Collapse multiple `Path.Combine` or `Path.Join` in a row

**Approved** | [#runtime/62057](https://github.com/dotnet/runtime/issues/62057#issuecomment-993892387) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=0h50m31s)

* This makes sense.
   - The category should be "Performance"
   - Severity should be "Info"
* We should special case using a single variable:
    ```C#
    string x = Path.Join("one", "two");
    x = Path.Join(x, "three");
    x = Path.Join(x, "file.txt");
    Console.WriteLine(x);
    ```
* Should this handle cases like this?
    ```C#
    _field = Path.Join("one", "two");
    _field = Path.Join(_field, "three");
    _field = Path.Join(_field, "file.txt");
    ```
* Do we have other examples of this pattern?
    - `String.Concat`
## DrawRectangle should accept RectangleF as one of the overloads

**Approved** | [#runtime/62401](https://github.com/dotnet/runtime/issues/62401#issuecomment-993897880) | [Video](https://www.youtube.com/watch?v=9IzfKxXviIU&t=1h2m24s)

* Seems like a no-brainer
    - Are there any other? Seems like `FillPie` would be in the same category.

```C#
namespace System.Drawing.Common
{
    public partial class Graphics
    {
        public void DrawRectangle(Pen pen, RectangleF rect);
        public void FillPie(Brush brush, RectangleF rect, float startAngle, float sweepAngle);
    }
}
```
