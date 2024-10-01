# API Review 10/01/2024

## It should be possible to specify `JsonIgnoreCondition`'s on the type level

**Approved** | [#runtime/108231](https://github.com/dotnet/runtime/issues/108231#issuecomment-2386577955) | [Video](https://www.youtube.com/watch?v=pxItI3kOOJU&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Text.Json.Serialization;

// Before:
//
// [AttributeUsage(AttributeTargets.Property |
//                 AttributeTargets.Field, AllowMultiple = false)]
//
[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Struct |
                AttributeTargets.Interface |
                AttributeTargets.Property |
                AttributeTargets.Field, AllowMultiple = false)]
public partial class JsonIgnoreAttribute;
```
## STJ should allow setting naming policies on the member level.

**Approved** | [#runtime/108232](https://github.com/dotnet/runtime/issues/108232#issuecomment-2386591306) | [Video](https://www.youtube.com/watch?v=pxItI3kOOJU&t=0h23m43s)

* Looks good as proposed
* @eiriktsarpalis should we update `JsonNumberHandlingAttribute` to support  `AttributeTargets.Interface`?

```C#
namespace System.Text.Json.Serialization;

[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Struct |
                AttributeTargets.Interface |
                AttributeTargets.Property |
                AttributeTargets.Field, AllowMultiple = false)]
public class JsonNamingPolicyAttribute : JsonAttribute
{
    public JsonNamingPolicy(JsonKnownNamingPolicy namingPolicy);
    protected JsonNamingPolicy(JsonNamingPolicy namingPolicy);
    public JsonNamingPolicy NamingPolicy { get; }
}
```

