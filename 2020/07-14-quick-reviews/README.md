# Quick Reviews 07/14/2020

## System.Text.Json: (De)serialization support for quoted numbers 

**Approved** | [#runtime/30255](https://github.com/dotnet/runtime/issues/30255#issuecomment-658313884) | [Video](https://www.youtube.com/watch?v=Cw-kYXQ7Sw8&t=0h0m0s)

* Looks good.
* We should have a discussion around defaults for .NET 5, especially web.
* We decided to rename `JsonNumberHandling.None` an `JsonNumberHandling.Strict`

```C#
namespace System.Text.Json
{
    public partial sealed class JsonSerializerOptions
    {
        public JsonNumberHandling NumberHandling { get; set; }
    }
}
namespace System.Text.Json.Serialization
{
    [Flags]
    public enum JsonNumberHandling
    {
        Strict = 0x0,
        AllowReadingFromString = 0x1,
        WriteAsString = 0x2,
        AllowNamedFloatingPointLiterals = 0x4
    }

    [AttributeUsage(
        AttributeTargets.Class |
        AttributeTargets.Struct |
        AttributeTargets.Property |
        AttributeTargets.Field, AllowMultiple = false)]
    public partial sealed class JsonNumberHandlingAttribute : JsonAttribute
    {
        public JsonNumberHandlingAttribute(JsonNumberHandling handling);
        public JsonNumberHandling Handling { get; }    
    }
}
```
## System.Text.Json: Ignore null values while serializing

**Approved** | [#runtime/39152](https://github.com/dotnet/runtime/issues/39152#issuecomment-658319929) | [Video](https://www.youtube.com/watch?v=Cw-kYXQ7Sw8&t=0h24m36s)

* Makes perfect sense.

```C#
public enum JsonIgnoreCondition
{
    // Never = 0,
    // Always = 1,
    // WhenWritingDefault = 2,
    WhenWritingNull = 3
}
```
