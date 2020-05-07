# Quick Reviews 5/7/2020

## Extend default value handling in JsonSerializer

**Approved** | [#runtime/35649](https://github.com/dotnet/runtime/issues/35649#issuecomment-625418116) | [Video](https://www.youtube.com/watch?v=CmF8g030Hno&t=0h0m0s)

* Looks great. Only suggestion is to change `WhenWritingDefaultValues` to `WhenWritingDefault`.
* `JsonSerializerOptions.DefaultIgnoreCondition` should throw `ArgumentException` rather than `InvalidOperationException`

```C#
namespace System.Text.Json.Serialization
{
    public enum JsonIgnoreCondition
    {
        Never = 0,
        Always = 1,
        WhenWritingDefault = 2
    }    
}

namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public JsonIgnoreCondition DefaultIgnoreCondition { get; set; } = JsonIgnoreCondition.Never;
    }
}
```
## Add mechanism to handle circular references when serializing

**Approved** | [#runtime/30820](https://github.com/dotnet/runtime/issues/30820#issuecomment-625444164) | [Video](https://www.youtube.com/watch?v=CmF8g030Hno&t=1h7m8s)

* The problem with taking a func is that people might accidentally close over a local variable that now becomes a static. Since the resolver is supposed to hold a dictionary this might result in two errors: (1) concurrency issues when multiple threads serialize and (2) a memory leak because the dictionary gets never eligible for GC6

```C#
namespace System.Text.Json.Serialization
{
    public partial class JsonSerializerOptions
    {
        public ReferenceHandler ReferenceHandler { get; set; }
    }
    public abstract class ReferenceHandler
    {
        public static ReferenceHandler Default { get; }
        public static ReferenceHandler Preserve { get; }
        public abstract ReferenceResolver CreateResolver();
    }
    public sealed partial class ReferenceHandler<T> : ReferenceHandler
        where T: ReferenceResolver, new()
    {
        public override ReferenceResolver CreateResolver() => new T();
    }
    public abstract class ReferenceResolver
    {
        public abstract void AddReference(string referenceId, object value);
        public abstract string GetReference(object value, out bool alreadyExists);
        public abstract object ResolveReference(string referenceId);
    }
}
```
